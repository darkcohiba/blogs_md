<h1>Email Integration In Ruby on Rails using Gmail!</h1>
<p>For my Capstone project at Flatiron School, I am working on building an application that allows users to post bottles they have for recycling and shelters can claim bottle postings. Claiming the bottle posts will send the user signed in an email with the location and photo of the bottles for pick up. This will allow the shelter to enable homeless people to get the bottles and turn them in for money at supermarkets or recycling plants. The idea is based off of the german system of recycling where bottles are valued more in society since people have to pay for the bottles when they buy the product.</p>

<p>As a basic introduction to sending emails based off of actions in Rails I learned how to set up a gmail account to send the emails and triggered an email when the user signed up for an account on my website. However, before we create the mailer and the mailer views, we need to create our basic database schema, our login, signup and logout functionality.</p>

<p>Now that we have our front end and back end set up we are going to add our user_mailer and our views. To generate the user_mailer type rails g mailer User , this will create two files, a user_mailer and a application_mailer. The application mailer will import from “ActionMailer::Base” and the user_mailer will inherent from the application mailer. Running the generate will automatically set up the inheritances. The application mailer has two fields,</p>

<img src="https://miro.medium.com/max/924/1*V99Xf3JvhdzPencbo5I02w.png">

<p>change the default field to the email address they will be sent from and leave the layout as ‘mailer’.</p>

<p>Now that we have our base we will set up our first email. This email will be a welcome email that triggers when a user signs up for our website and creates a new username. We will call this email welcome_email and we will create a method in our user_mailer file called welcome_email. In our method we will declare two varaibles, one for our user and one for the url of the login page of our website. After these variables we will create the mail action:</p>

<img src="https://miro.medium.com/max/1192/1*X_QVm07UBmz9urXr6cs1_g.png">

<p>The mail action includes a to and the subject line. I am sending it to the users email who signs up and the subject is welcome to my website.</p>

<p>The next step is to create the html and text formats for the email, they will go within the views folder under a ‘user_mailer’ folder. You will need to create two versions of the email, a text and an html format. This folder layout will look like;</p>

<img src="https://miro.medium.com/max/404/1*UPO4_-PsavgiSdbEMJ_tmg.png">

<p>The emails can have anything you want inside them and can use the params that were passed in via the method that matches the views title. For my HTML email I have the following:</p>

<img src="https://miro.medium.com/max/1236/1*dS14cxLOXX5bWTZ0F30HtQ.png">

<p>For my text email, I have the same thing as my html but in a different format:</p>

<img src="https://miro.medium.com/max/1020/1*QXb4BUubKe57SS4hzbzwQg.png">

<p>Perfect! Now we have our email set up, the last two steps are to add a trigger and integrate our gmail account with the mailer.</p>

<p>To trigger the mailer we are going to add a line to the controller crud action that will set off our mailer. For this example we are sending the email when new user is created. Within the users_controller, we have our create action that posts a new user account. We are going to add this line inside the method for create if the user is saved,</p>

```ruby
UserMailer.with(user:@user).welcome_email.deliver_later

```

<p>We are calling the Usermailer we created with the new user variable information. Then we are calling the welcome_email method we created and delivering the email asynchronously after the crud action is completed. We can also deliver_now if we want the crud action to pause until after the email is sucessfully saved. Now that our trigger, and email is set up we just need to set up our gmail account integration so our guest will recieve the welcome email from our existing gmail account.</p>

<p>To do this we will add this block of code to our production and development enviroments inside the config folder.</p>

<img src="https://miro.medium.com/max/1146/1*99GkP0iD-r6XqVMozoFgBA.png">

<p>First we are setting the default url options host, this is going to be the IP address of the computer you are using and the port you are pushing it from or the link for your deployed website. Next we are setting the delivery method to ‘smtp’ and thus have to update our smtp settings to match our google port. Within smtp settings, we only need to adjust two settings, our user_name and password should match the credentials for the google account you are using to send the emails. Once this is completed, I prefer to turn on the actions within the enviroments that allow rails to communicate errors when sending emails. After this is complete you are ready to test your emails!</p>

<p>Open your login/signup page, create a new account and see if your emails were sent! If everything works the new user will recieve an email like this:</p>

<img src="https://miro.medium.com/max/1106/1*TO6lf3YUXd1inEK49iBVzQ.png">

<p>Of course you can run into a littany of errors and bumps along the way but a welcome email is a great first step to adding email integrationt through gmail to any project.</p>

<p>Thanks for reading!</p>

<h3>Sources:</h3>
<ul>
    <li><a href="https://guides.rubyonrails.org/action_mailer_basics.html">https://guides.rubyonrails.org/action_mailer_basics.html</a></li>
</ul>