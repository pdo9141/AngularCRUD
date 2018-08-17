Check Node version: node -v
Check Angular CLI version: ng -v
From cmd open project in VS Code: code .

Angular 5 Setup
01) Create a new folder:  mkdir ng-5-cli && cd ng-5-cli
02) Initialise NPM npm init (cancel after)
03) Install older Angular CLI: npm i @angular/cli@1.7.4 --save-dev
04) For some reason now the ng compiler assumes this is a angular project directory and does not allow running ng new here.
05) Step out of the current directory: cd ..
06) Use the absolute path to Angular cli in the directory from step #1.
    For example C:\temp\ng-5-cli\node_modules\.bin\ng new some-ng5-app

Angular 6 Setup
01) Install latest version of Node
02) Install Angular CLI: npm install -g @angular/cli    
03) Create new Angular project: ng new AngularCrud --skip-tests true
04) Install Bootstrap: npm install bootstrap@3 --save
05) Add reference of Bootstrap in angular-cli.json
    Add "../node_modules/bootstrap/dist/css/bootstrap.min.css" in apps/styles (above style.css)
06) Run our project: ng serve -o    
07) To create component: ng g c employees/listEmployees --spect false --flat true

https://www.youtube.com/watch?v=pcOaAU_iaD4&list=PL6n9fhu94yhXwcl3a6rIfAI7QmGYIkfK5&index=3