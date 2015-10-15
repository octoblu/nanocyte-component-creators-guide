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
Envelope is blah blah blah and has three important properties:

`envelope.message`: This will be used if you want to use the entire message that was passed in. For example, the 'Equal' component uses it's `config` to check whether or not to pass on the `message`.

`envelope.config`: This will be used if your component has any configurations. For example, the 'Pluck' component's `config` consists of a 'key' and 'value'.

`envelope.data`: This will be used if your component needs to keep any data within itself. For example, the 'Collect' component uses `data` as an array and pushes values from the `config` to it.

## Adding to registry???

### Registry component options
- ”type": "nanocyte-component-pass-through",
- ”linkedTo": ["check"]
- ”linkedToPrev": Boolean 
- ”linkedToNext”: Boolean (links to next node)
- ”linkedToOutput": Boolean  (Outputs to Meshblu)
- ”linkedToData": Boolean (Stores the state of node) 
- ”linkedFromStop": Boolean (links to the stop event of the flow)
- ”linkedFromStart": Boolean (links to the start event of the flow)
- ”linkedToPulse": Boolean