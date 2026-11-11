# Java Registration Form

A Java Swing desktop application that shows a student registration form, saves the submitted data to a MySQL database via JDBC, and sends a confirmation email via Gmail SMTP using JavaMail.

## Features

- Form fields: name, gender (Male/Female drop-down), father's name, password, confirm password, city and email.
- Password check: the record is only saved if the password and confirm-password fields match; otherwise a "Password did not match" dialog is shown.
- Database storage: on **REGISTER**, a row is inserted into the `student` table using a `PreparedStatement`.
- Confirmation email: after a successful insert, a "Registration Confirmation" email is sent to the address entered in the form.
- **RESET** clears all fields.

## Tech stack

- Java with Swing (GUI) and JDBC
- MySQL with [MySQL Connector/J](https://dev.mysql.com/downloads/connector/j/) (driver class `com.mysql.cj.jdbc.Driver`)
- JavaMail (`javax.mail`) for sending email over `smtp.gmail.com:587` with STARTTLS

## Repository contents

| File | Description |
| --- | --- |
| `RegistrationForm.java` | The whole application (package `registrationform`, class `RegistrationForm1`) |
| `.github/workflows/gradle-publish.yml` | GitHub Actions workflow that runs `gradle build` / `gradle publish` when a release is created |
| `LICENSE` | Apache License 2.0 |

Note: the repository has no `build.gradle`, so the Gradle workflow cannot build the project as-is (needs confirmation / a Gradle build file).

## Requirements

- JDK 8 or later
- A running MySQL server
- MySQL Connector/J JAR
- JavaMail JAR (`javax.mail`). On JDK 11+ the JavaBeans Activation Framework JAR may also be required (needs confirmation for your JavaMail version).

## Database setup

The code inserts into a table named `student` with these columns:

`name`, `gender`, `fatherName`, `password`, `city`, `email`

Column types are not defined in the repository; an example schema:

```sql
CREATE TABLE student (
  name       VARCHAR(100),
  gender     VARCHAR(10),
  fatherName VARCHAR(100),
  password   VARCHAR(100),
  city       VARCHAR(100),
  email      VARCHAR(100)
);
```

Passwords are stored as plain text by the current code.

## Configuration

There are no environment variables or config files; values are hard-coded placeholders in `RegistrationForm.java` that you must edit:

| Location | Placeholder | Replace with |
| --- | --- | --- |
| `actionPerformed` | `jdbc:mysql://localhost:3306/your_database` | Your JDBC URL / database name |
| `actionPerformed` | `"username"`, `"password"` | Your MySQL user and password |
| `sendEmail` | `senderEmail = "your_email"` | Gmail address used to send emails |
| `sendEmail` | `senderPassword = "your_password"` | Password / app password for that account |

Do not commit real credentials. For anything beyond local testing, load them from environment variables or a config file instead.

## Build and run

The source declares `package registrationform;` and `public class RegistrationForm1`, while the file is named `RegistrationForm.java`. Java requires a public class to live in a file with the same name, so the file must be placed as `registrationform/RegistrationForm1.java` before compiling (or opened in an IDE, which will flag the same issue).

From the repository root, with the two JARs downloaded (replace the JAR names with the files you have):

```sh
mkdir -p registrationform
cp RegistrationForm.java registrationform/RegistrationForm1.java

# Linux/macOS (use ';' instead of ':' as the classpath separator on Windows)
javac -cp "mysql-connector-j.jar:javax.mail.jar" registrationform/RegistrationForm1.java
java  -cp ".:mysql-connector-j.jar:javax.mail.jar" registrationform.RegistrationForm1
```

The registration window opens; fill in the form and click **REGISTER** to save the record and send the confirmation email, or **RESET** to clear it.

## Contributing

Contributions are welcome: fork the repository, make your changes on a branch and open a pull request.

## License

This project is licensed under the [Apache License 2.0](LICENSE).
