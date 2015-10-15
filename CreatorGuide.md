# Nanocyte component creator's guide

## What are nanocyte components?
Nanocyte components are lightweight nodes that can perform custom operations on your data in Octoblu. By creating your own nanocyte components, you can exert even more control over your IoT devices.

## Generating your component
Before you can generate a nanocyte component, you will need to:
```
npm install generator-nanocyte-component
```
Create a new directory, such as `nanocyte-component-example`, and `cd` into it.

Now you can generate your nanocyte component by:
```
yo nanocyte-component
```

## Creating your component
After you have generated your component, you will need to edit the source file. For this example, it is located under `nanocyte-component-example/src/example.coffee`

You can now start writing your component, however there are a few things about the `envelope` variable that need to be considered.

### What is envelope?
Envelope is blah blah blah and has three properties:

`envelope.message` -

`envelope.config` -

`envelope.data` -

## Adding to registry???
