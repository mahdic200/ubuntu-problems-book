# expo not working on phone

## problem

If you have encountered this problem lately it is because you have installed expo-dev-client package which prevents the `npx expo start` command to publish an Ip address and uses localhost instead .

## solution

Remove the expo-dev-client package and also remove the eas (or install them manually whenever you want to make a local build) .

