# GrowthWebsite

- Default HTML based website for showcasing Growth Tutorials.

- Also includes detailed overview of SaaS application - GT Gurukul

# Working pattern - Keep it Agile!
- Add next to do here - whatever is in mind
- Share this md file with chat GPT and ask to summarize it in md format and update here
- Discuss in new chat in chat gpt for any new to do that is taken up
- Create template first for the next item taken up - always check for best design
- Plan Firestore structure and then integrate UI + Firestore
- On completion of the to do, summarize and update here
- Make sure to create quick usable items, deploy, take feedback/explore and improvise

# Next To-Do for Growth Tutorials SaaS-based Application

## Activity tracker
- Count of active logged-in users in gt_aj_app via common component --  either via pre build firestore auth (if possible) or by saving info whenever login happens
- Total users list to be showns to admin in gt_aj_register -- create a side menu for admin so that switch can be done between Institute list and Users list

## Can Git md file be displayed in website?

## Add News & Events Module
- Develop a module to manage and display news and events related to the institute.
- Include functionalities for adding, editing, and deleting news items and events.
- Ensure the module is integrated with the main dashboard for easy access and visibility.

## Implement Lecture & Attendance Management
- Create a module for scheduling and managing lectures.
- Develop an attendance tracking system that allows teachers to mark attendance and view attendance history.
- Provide reporting features for attendance data.

## Add Quiz and Report Modules
- Implement a quiz module where teachers can create, assign, and grade quizzes.
- Develop a report module that generates performance reports for students based on quizzes, attendance, and other metrics.

## Activity Log Tracking and Display
- Develop a system to track user activities within the application.
- Create a module to display activity logs to administrators for monitoring purposes.
- Ensure activity logs include important actions such as login, logout, data modifications, and other key interactions.

## Enhance Registration Page
- Pre-fill the institute name field based on the subdomain parameter.
- Provide a smooth user experience for registering new institutes.

## Add Additional Authentication Methods
- Implement additional authentication mechanisms (e.g., Facebook, Twitter) alongside Google authentication.

## Improve UI and UX
- Refine the registration process to ensure data is correctly saved in Firestore.
- Continue using Angular Material components to enhance UI consistency and accessibility.

## Optimize Firestore Usage
- Implement more efficient Firestore reads and writes to stay within the free tier limits.
- Consider implementing Firestore security rules that balance security and performance.

## Implement Cache Invalidation Strategy
- Develop a strategy to invalidate cached data when necessary to ensure users always have up-to-date information.

## Enhance Error Handling and User Feedback
- Improve error messages and feedback for users during the registration and customization processes.
- Ensure that all potential error states are handled gracefully.

## Testing and Quality Assurance
- Conduct thorough testing to ensure all functionalities work as expected.
- Implement automated tests for critical components to ensure ongoing reliability.

## Documentation and Onboarding Guides
- Create comprehensive documentation for both users and developers.
- Develop onboarding guides and interactive tutorials to help new users get started with the application.

## Monitor and Analyze Usage
- Set up analytics to monitor user behavior and application performance.
- Use insights from analytics to continuously improve the application.

## Prepare for Deployment
- Ensure all aspects of the application are ready for deployment.
- Plan and execute a deployment strategy, including backup and rollback procedures.

By incorporating these additional modules and functionalities, the Growth Tutorials SaaS-based application will offer a comprehensive suite of tools for educational management, enhancing its value and utility for users.

# Summary of Activities Done for Growth Tutorials SaaS-based Application:

## Firebase Project Setup:
- Created a Firebase project and added a web app named gt_aj_register.
- Configured the Angular application with Firebase authentication and Firestore.
- Utilized the institute name as the document ID in the Firestore collection to store registration-related details.

## User Roles and Firestore Integration:
- Set up user roles in Firestore with collections for users, registrations, and institutes, each containing relevant subcollections and documents.
- Implemented super user role functionality in Firestore and Angular.
- Updated Firestore rules to manage access based on user roles.
- Created a service in Angular to manage user roles and institute statuses.
- Updated the UI to allow super users to enable or disable institute statuses.

## UI Development and Docker Setup:
- Worked on the UI development for Growth Tutorials' SaaS application registration page, named GT Gurukul.
- Added Docker to the NGINX setup on AWS EC2 Amazon Linux 2023 server. >>> * Further implementation of docker is still pending *
- Configured NGINX with default 80 and HTTPS ports, linking subdomains to the appropriate Angular apps.

## Customization and Theming:
- Implemented a service to fetch customization data from Firestore, cache it in local storage, and integrate it into the Angular application to dynamically set the browser tab title and theme color.
- If Firestore does not find the subdomain as registered, the application redirects to the registration page with the subdomain as a parameter.

## Error Handling and Redirection:
- Managed error handling by ensuring that if the subdomain is not registered, the user is redirected to a registration page with the subdomain prefilled for a smoother registration process.
- Addressed the issue of error being of type unknown by casting it to a known type (any).
