[![Build Status](https://travis-ci.org/alexm118/presentation-demo.svg?branch=master)](https://travis-ci.org/alexm118/presentation-demo)[![Coverage Status](https://coveralls.io/repos/github/alexm118/presentation-demo/badge.svg?branch=master)](https://coveralls.io/github/alexm118/presentation-demo?branch=master)

This is demonstration repository for configuring a React application to deploy to Heroku through Travis CI, while also sending Code Coverage to Coveralls.

# Requirements #
  * Github Account
  * [Heroku](https://dashboard.heroku.com) Connected to your Github Account
  * [Travis CI](https://travis-ci.org) Connected to your Github Account
  * [Coveralls](https://coveralls.io) Connected to your Github Account
  * [Create React App](https://github.com/facebookincubator/create-react-app)

## Getting Started ##
To get started we need to create a new React application using create-react-app.
```sh
create-react-app $APP_NAME
```
Now we need to eject our app to free itself up
```sh
npm run eject
```
Now we can install the coveralls package for our application
```sh
npm install --save-dev coveralls
```
Now we need to commit these changes to a new git repo
```sh
git init
git add .
git commit -m "First Commit"
```
Create a new Repository on Github and then run the following commands
```sh
git remote add origin $GITHUB_URL
git push -u origin master
```
## Deploying to Heroku ##
Now we can connect the Repo to Heroku after creating a new application
  * Navigate to the Deploy tab, and select Github as your deployment method
  * Select the Github repository for your react application.
  * Enable automatic deploys
  * Navigare to the Settings tab
  * Under Buildpacks add the following URL: https://github.com/mars/create-react-app-buildpack

Now we can push a new commit and watch it deploy on Heroku.
Lets change our App.js file
```javascript
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h1 className="App-title">Welcome to React</h1>
        </header>
        <p className="App-intro">
          Deployed to Heroku!
        </p>
      </div>
    );
  }
}

export default App;
```
Now the simple commit
```sh
git commit -am "Watch me deploy to heroku!"
```

## Utilizing Coveralls and Travis CI ##
Now that we have deployed to Heroku we can use Travis CI to run our tests and build our application.
  * First up, configure Travis CI to listen to your Github repo. 
  * Note - you will most likely need to re-sync your github account and refresh the page to toggle your repo on.
  * Now that we've turned on Travis CI to listen to the repo, we can add a travis.yml file
  
```yml
language: node_js
node_js:
- stable
cache:
  directories:
  - node_modules
script:
- npm test
after_success:
- cat ./coverage/lcov.info | ./node_modules/coveralls/bin/coveralls.js
```
