# Nanocyte component creator's guide

## What are nanocyte components?
Nanocyte components are lightweight nodes that can perform custom operations on your data in Octoblu. By creating your own nanocyte components, you can exert even more control over your IoT devices.

## Generating your component
Before you can generate a nanocyte component, you will need to:
```
npm install -g yo
npm install -g generator-nanocyte-component
```
_'generator-nanocyte-component' is Octoblu's 'yo' component generator. Yeoman (yo) is a simple tool that will run your generators._

Create a new directory, such as `nanocyte-component-example`, and `cd` into it.

Now you can generate your nanocyte component by:
```
yo nanocyte-component
```

## Creating your component
After you have generated your component, you will need to give your component functionality by editing the source file. For this example, it is located under `nanocyte-component-example/src/example.coffee`

You can now start writing your component, however there are a few things about the `envelope` variable that need to be considered...

### What is envelope?
Envelope will be available for all nanocyte components and has three important properties:

`envelope.message`: This will be used if you want to use the entire message that was passed in. For example, the 'Equal' component uses it's `config` to check whether or not to pass on the `message`.

`envelope.config`: This will be used if your component has any configurations. For example, the 'Pluck' component's `config` consists of a 'key' and 'value'.

`envelope.data`: This will be used if your component needs to keep any data within itself. For example, the 'Collect' component uses `data` as an array and pushes values from the `config` to it.

## Adding to registry
After you have created your nanocyte component, you will need to add it the the `nanocyte-node-registry`. To do this, you will need to first clone the repository:
```
git clone https://github.com/octoblu/nanocyte-node-registry.git
```
Now you will need to edit the registry to include your component. The registry is located in `nanocyte-node-registry/registry.json`. There are a variety of properties to keep in mind when adding your component to the registry.

### Registry component properties
`"composedOf"`|`Object`: This will be used by every component and tells the registry how to build your component. For example, the 'greater-than-equal' component is actually built in the registry by combining the functionality of the 'greater-than' and 'equal' components:
```
"greater-than-equal": {
  "composedOf": {
    "greater-than": {},
    "equal": {}
  }
}
```

`”type"`|`String`: This will also be used by every component and is the complete name of your component. For example:
```
"example": {
  "composedOf": {
    "example": {
      "type": "nanocyte-component-example"
    }
  }
}
```

`"sendWhitelist"`|`Array`: This will be used if you need to send messages to a specific device. For example, our 'delay' component uses our interval service, which is a device with a UUID:
```
"example": {
  "sendWhitelist": [super-awesome-uuid, another-awesome-uuid],
  "composedOf": {}
}
```

`”linkedTo"`|`Array`: This will be used if you need to chain components together. For example, our 'rss' component has multiple steps: format, request, parse:
```
"rss": {
  "composedOf": {
    "http-formatter": {
      "type": "nanocyte-component-http-formatter",
      "linkedToPrev": true,
      "linkedTo": ["http-request"]
    },
    "http-request": {
      "type": "nanocyte-component-http-request",
      "linkedTo": ["parse-body"]
    },
    "parse-body": {
      "type": "nanocyte-component-parse-body",
      "linkedToNext": true
    }
  }
}
```

`”linkedToPrev"`|`Boolean`: This will be used if you need to tell your component to allow other nodes to connect to it as input. There is an example in the next property.

`”linkedToNext”`|`Boolean`: This will be used if you need to tell your component to allow itself to connect to other nodes as output. For example, this component can take input and send output:
```
"example": {
  "composedOf": {
    "example": {
      "type": "nanocyte-component-example",
      "linkedToPrev": true,
      "linkedToNext": true
    }
  }
}
```

`”linkedFromStart"`|`Boolean`: This will be used if you need to tell your component to do something when your flow deploys, or starts. There is an example below.

`”linkedFromStop"`|`Boolean`: This will be used if you need to tell your component to do something when your flow stops. There is an example below.

`”linkedToOutput"`|`Boolean`: This will be used if you need to send your output to Meshblu. For example, the 'flow-metrics' component waits for flows to start or stop and sends it's data to Meshblu:
```
"flow-metrics": {
  "composedOf": {
    "flow-metric-start": {
      "type": "nanocyte-component-flow-metric-start",
      "linkedFromStart": true,
      "linkedToOutput": true
    },
    "flow-metric-stop": {
      "type": "nanocyte-component-flow-metric-stop",
      "linkedFromStop": true,
      "linkedToOutput": true
    }
  }
}
```

`”linkedToData"`|`Boolean`: This will be used if you need to set data on your component, using it's `envelope.data` property. For example:
```
"example": {
  "composedOf": {
    "example": {
      "type": "nanocyte-component-example",
      "linkedToPrev": true,
      "linkedToData": true
    }
  }
}
```

`”linkedToPulse"`|`Boolean`: This will be used if you need to make your component pulse in the Octoblu web app. For example, this component consists of other components, but only needs to pulse once it finishes:
```
"example": {
  "composedOf": {
    "step-one": {
      "type": "nanocyte-component-step-one",
      "linkedToPrev": true,
      "linkedTo": ["step-two"]
    },
    "step-two": {
      "type": "nanocyte-component-step-two",
      "linkedTo": ["step-three"]
    },
    "step-three": {
      "type": "nanocyte-component-step-three",
      "linkedToNext": true,
      "linkedToPulse": true
    }
  }
}
```

## Submitting a pull request
