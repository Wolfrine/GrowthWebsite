[<- Back](../overview.md)

## 3. Project Structure

**Explanation of the Folder Structure:**

The GT Gurukul project follows a standard Angular application structure with some additional custom folders for specific functionalities. Below is an overview of the key folders and files:

```
gt-gurukul/
├── e2e/                        # End-to-end tests
├── node_modules/               # Project dependencies
├── src/                        # Source files for the application
│   ├── app/                    # Main application folder
│   │   ├── components/         # Angular components
│   │   ├── services/           # Angular services
│   │   ├── guards/             # Route guards
│   │   ├── app-routing.module.ts # Application routing module
│   │   ├── app.component.html  # Main application template
│   │   ├── app.component.ts    # Main application component
│   │   ├── app.module.ts       # Main application module
│   ├── assets/                 # Static assets (images, fonts, etc.)
│   ├── environments/           # Environment configurations
│   │   ├── environment.ts      # Development environment
│   │   ├── environment.prod.ts # Production environment
│   ├── styles/                 # Global styles
│   ├── index.html              # Main HTML file
│   ├── main.ts                 # Main entry point for Angular
│   ├── polyfills.ts            # Polyfills for browser compatibility
│   └── test.ts                 # Test configuration
├── .angular-cli.json           # Angular CLI configuration
├── .editorconfig               # Editor configuration
├── .gitignore                  # Git ignore file
├── angular.json                # Angular project configuration
├── package.json                # NPM package configuration
├── README.md                   # Project documentation
└── tsconfig.json               # TypeScript configuration
```


**Key Files and Their Purposes:**

- **app.module.ts:** Main module of the application where all components, services, and other modules are declared and imported.
- **app-routing.module.ts:** Defines the routes for the application.
- **environment.ts:** Configuration for the development environment.
- **environment.prod.ts:** Configuration for the production environment.
- **main.ts:** Entry point of the application, bootstraps the main module.
- **styles.scss:** Global styles for the application.
- **index.html:** The main HTML file that serves the Angular application.
