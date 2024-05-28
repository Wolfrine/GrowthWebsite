# GrowthWebsite

- Default HTML based website for showcasing Growth Tutorials.

- Also includes detailed overview of SaaS application - GT Gurukul

## Summary of Activities Done for Growth Tutorials SaaS-based Application:
1. Firebase Project Setup:
  - Created a Firebase project and added a web app named gt_aj_register.
Configured the Angular application with Firebase authentication and Firestore.
Utilized the institute name as the document ID in the Firestore collection to store registration-related details.
User Roles and Firestore Integration:

Set up user roles in Firestore with collections for users, registrations, and institutes, each containing relevant subcollections and documents.
Implemented super user role functionality in Firestore and Angular.
Updated Firestore rules to manage access based on user roles.
Created a service in Angular to manage user roles and institute statuses.
Updated the UI to allow super users to enable or disable institute statuses.
UI Development and Docker Setup:

Worked on the UI development for Growth Tutorials' SaaS application registration page, named GT Gurukul.
Added Docker to the NGINX setup on AWS EC2 Amazon Linux 2023 server.
Configured NGINX with default 80 and HTTPS ports, linking subdomains to the appropriate Angular apps.
Customization and Theming:

Implemented a service to fetch customization data from Firestore, cache it in local storage, and integrate it into the Angular application to dynamically set the browser tab title and theme color.
If Firestore does not find the subdomain as registered, the application redirects to the registration page with the subdomain as a parameter.
Error Handling and Redirection:

Managed error handling by ensuring that if the subdomain is not registered, the user is redirected to a registration page with the subdomain prefilled for a smoother registration process.
Addressed the issue of error being of type unknown by casting it to a known type (any).
