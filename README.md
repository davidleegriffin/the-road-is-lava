deadreads - Social Media for Horror Book lovers
By Adam Jacobson Zac Watts Michael Gann David Griffin

[Table Of Contents]:
Description
Application Architecture && Technologies Used
Technology Shields
Frontend Overview
Backend Overview
[Description]:
Deadreads, a goodreads clone, is a social media application that allows users to review horror books, add horror books to bookcrypts along with, view reviews and profiles of other horror book lovers.

Get skull𝔰քօօӄʏskull.

[Application Architecture && Technologies Used]:
Deadreads was built using the Express NodeJS framework with a PostgreSQL(postgres) database to store all the application data in combination.

The frontend uses the Pug templating engine to render views from the frontend server. We used vanilla Javascript for interactivity and standard CSS for styling as well.

[Frontend Overview]:
Most of the application was based on the backend and its database. Although there was a fairly heavy usage of the Pug template engine to create a more dynamic HTML page. The navigation should be easy and intuitive with the user having the ability to customize his/her/their experience.

[Example Sign-up]
An example of the sign-up form and the code for it:

router.post(
  "/sign-up",
  csrfProtection,
  userValidators,
  asyncHandler(async (req, res) => {
    const { username, email, birthdate, gender, fullName, password } = req.body;
    const hashedPassword = await bcrypt.hash(password, 10);

    const user = await db.User.create({
      username,
      email,
      birthdate,
      gender,
      fullName,
      hashedPassword,
    });

    const hasReadCrypt = await db.Crypt.create({
      name: "Have Read",
      userId: user.id,
    });

    const wantsToReadCrypt = await db.Crypt.create({
      name: "Want to Read",
      userId: user.id,
    });

    const currentlyReadingCrypt = await db.Crypt.create({
      name: "Currently Reading",
      userId: user.id,
    });

    const validatorErrors = validationResult(req);
    if (validatorErrors.isEmpty()) {
      loginUser(req, res, user);
      return req.session.save(() => {
        res.redirect("/");
      });
    } else {
      const errors = validatorErrors.array().map((error) => error.msg);
      res.render("sign-up-form", {
        title: "New User",
        user,
        errors,
        csrfToken: req.csrfToken(),
      });
    }
  })
);
Database Schema

[Example Login]:
If the user wishes to try the sites functionality before signing up, select the demo user button. An example of how to use the login function, and the code we used to implement it:

router.post(
  "/login",
  loginValidators,
  asyncHandler(async (req, res) => {
    const { username, password } = req.body;

    let errors = [];

    const validatorErrors = validationResult(req);

    if (validatorErrors.isEmpty()) {
      const user = await db.User.findOne({
        where: {
          username,
        },
      });
      if (user !== null) {
        const passwordMatched = await bcrypt.compare(
          password,
          user.hashedPassword.toString()
        );
        if (passwordMatched) {
          loginUser(req, res, user);
          return req.session.save(() => {
            res.redirect("/");
          });
        }
      }
      errors.push("Login failed for the provided username and password.");
    } else {
      errors = validatorErrors.array().map((error) => error.msg);
    }
    res.render("log-in-form", {
      title: "Login",
      username,
      errors,
    });
  })
);
Database Schema

[Example of Site Navigation]
A display of various routes taken while navigating site Database Schema

[Backend Overview]:
Our application is fairly heavy with using the database we setup. We used Sequelize in order to create our database. We also used csurf on the frontend in combination with bcrypt to get user authentication to work how we wanted.

[Relational Database Model]:
One of the larger challenges of this project was ensuring we had the right database setup. We tried to design it well enough to cover all of the usages we needed. This project was incredibly important to have the relations between tables setup properly.

The end database schema:
Database Schema

[Conclusion && Next Steps]:
Things learned while we worked on deadreads:

Some issues are immensely simple, but after long hours of programming you miss them.
Pug is very particular about tags.
More than one brain and one set of eyes is always better for debugging.
Going back to the project with a fresh mind always helps.
Next Steps:

Add a list of a users reviews page
Clean up the profile page
[Technology Shields]:
        
