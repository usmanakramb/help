# Migrate using the API

If you prefer to create your entities from scratch and migrate using the API, you can use this procedure. You will need to create a SpotDeployment YAML and the entities.

## Step 1: Create and Apply Entities

Create a SpotDemployment YAML, create your entities, and apply them accordingly, using Postman or another tool you prefer.

Note the following:
- When you apply a SpotDeployment for the first time, Ocean CD creates the first replicas, but will **not** trigger a deployment.
- The Rolloutspec triggers the Strategy, so you will need to first create a strategy, and only then the Rolloutspec.

YAML templates and examples can be found in the [Ocean CD public repository](https://github.com/spotinst/spot-oceancd-releases/tree/main/Quick%20Start%20%26%20Examples) or JSON syntax via the [Ocean CD API](http://docs.spot.io/api).

## Step 2: Trigger Canary Deployment

To trigger a canary deployment, make one or more changes to the SpotDeployment you created and apply them. 

## What’s Next?

Learn more about the different [traffic manager options](ocean-cd/getting-started/traffic-manager-reference) available.