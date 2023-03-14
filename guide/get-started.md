---
description: Get started with a free parallelization using sorry-cypress
---

# Get Started

Let's start by running a basic sorry-cypress configuration:

```
docker run -p 1234:1234 agoldis/sorry-cypress-director
```

We've just launched `director` service on [`http://localhost:1234`](http://localhost:1234) - this service coordinates cypress agents and enables free parallelization.

### Install and configure cypress-cloud and cypress

``[`cypress-cloud`](https://github.com/currents-dev/cypress-cloud) is an open-source tool that allows integrating Cypress with alternative cloud services like Currents or Sorry Cypress.

```bash
npm install cypress-cloud cypress
```

Create a new configuration file: `currents.config.js` in the project’s root, set the `projectId` and the record key obtained from [Currents](https://app.currents.dev/) or your self-hosted instance of Sorry Cypress

```javascript
// currents.config.js
module.exports = {
  projectId: "Ij0RfK", // the projectId 
  recordKey: "xxx", // the record key, can be any values for sorry-cypress users
  // Sorry Cypress users - set the director service URL
  cloudServiceUrl: "http://cy.currents.dev",
};
```

Add `cypress-cloud/plugin` to `cypress.config.{js|ts|mjs}`

```javascript
// cypress.config.js
const { defineConfig } = require("cypress");
const { cloudPlugin } = require("cypress-cloud/plugin");
module.exports = defineConfig({
  e2e: {
    setupNodeEvents(on, config) {
      return cloudPlugin(on, config);
    },
  },
});
```

### Running cypress tests in parallel <a href="#running-cypress-tests-in-parallel" id="running-cypress-tests-in-parallel"></a>

Let's open several terminal windows and run `cy2` in each. Make sure you have cypress tests defined in advance (clone [https://github.com/agoldis/sorry-cypress-demo.git](https://github.com/agoldis/sorry-cypress-demo.git) if you don't have any tests handy).

```bash
# run in each terminal
npx cypress-cloud run --parallel --record --key somekey --ci-build-id hello-cypress
```

You'll notice that different instances of cypress agents are running different tests.&#x20;

🎉 We've just finished the basic setup of sorry-cypress and ran our tests in parallel!

{% hint style="info" %}
* Use the same `--ci-build-id` to associate different cypress agents with the same run
* You can run as many [cypress agents](../concepts/parallelization-guide.md) as you want - each  will run a different test suite
* This basic `director` configuration keeps all the test results in memory. Restarting it wipes all the data
* `--key` and `projectId` do not have any effect on the basic setup
{% endhint %}
