# environement-in-angular

## 环境变量在Angular中的应用

> If you are building a normal web app which need a backend server such as express/apache, using restful API is common.

> But you will want to use your test personal api  during development and then your real api in production. It’s easy to do in Angular using the `environment.ts` file.

> Angular CLI projects already use a production environment variable to enable production mode when in the production environment, which is showing in main.ts

```js
// ...
// this will enable prod environment
if (environment.production) {
  enableProdMode();
}
// ...
```

> And you’ll also notice that by default in the `/src/environment` folder you have an environment file for development and one for production.

> let's say we want to use different api address depending if we are in devlopment or production mode.

### environment.ts

```js
// localhost:3000 is common port for node server
export const environment = {
  production: false,
  ipAddress: localhost:3000
};
```

### environment.prod.ts

```js
// ipaddress maybe the real address for your web-app
export const environment = {
  production: true,
  ipAddress: 100.100.100
};
```

## how to use environment variable in your component

> All you have to do to use environment in your comoponent is quiet straight and folliwing:

```js
import { Component } from '@angular/core';
import { environment } from '../environments/environment';

@Component({ ... })
export class AppComponent {
  animal: string = environment.animal;
}
```

> Even though it looks like the environment variable extract from environment file in environments folder.

> But actually Angular takes care of `swapping the environment file` for the correct one.

> Now in development mode the ipAddress variable resolves to `localhost:3000` and in production, if you run `ng build --prod`(also can be `ng build` directly) for example, ipAddress resolves 100.100.100.

## Detecting Development Mode With isDevMode() function

> Just in case, Angular also provides developers  with an utility function called `isDevMode` that makes it easy to check if the app in running in dev mode:

```js
import { Component, OnInit, isDevMode } from '@angular/core';

@Component({ ... })
export class AppComponent implements OnInit {
  ngOnInit() {
    if (isDevMode()) {
      console.log('Development mode');
    } else {
      console.log('Production mode');
    }
  }
}
```

## Adding a new environment

> It’s easy to add new environments in Angular CLI projects by adding new entries to the `configurations` field in the `angular.json` file(with customized properties). Let’s add a new environment for example:

```json
//...
"configurations": {
            "production": {
              "fileReplacements": [
                {
                  "replace": "src/environments/environment.ts",
                  "with": "src/environments/environment.prod.ts"
                }
              ],
              "optimization": true,
              "outputHashing": "all",
              "sourceMap": false,
              "extractCss": true,
              "namedChunks": false,
              "aot": true,
              "extractLicenses": true,
              "vendorChunk": false,
              "buildOptimizer": true
            },
            "testEnvironment": {
                "fileReplacements": [
                    {
                        "replace": "src/environments/environment.ts",
                        "with": "src/environments/environment.test.ts"
                    }
                ],
                "optimization": true,
                "outputHashing": "all",
                "sourceMap": false,
                "extractCss": true,
                "namedChunks": false,
                "aot": true,
                "extractLicenses": true,
                "vendorChunk": false,
                "buildOptimizer": true
            }
          }
//...
```

> And now we can add a test environment file in environment folder and suddenly use another ipAddress if we build the project with `ng build --env=testEnvironment`:

### environment.testEnvironment.ts

```js
export const environment = {
  production: true,
  ipAddress: 111.111.111
};
```

> But usually, you do not need to create a new environment file in you project exclude those specific situations.