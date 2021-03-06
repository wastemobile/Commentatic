# Commentatic, static comments for Statamic

Use the magic of Statamic to power your comments

## Features

- Basic supports authentication of [Statamic](http://statamic.com/)
- Gravatar Support
- Honeypot (inspired by Eric Barnes (<https://github.com/ericbarnes/Statamic-email-form>)

## Installation & Config

Copy first the commentatic.yaml.sample file from the plugin folder to _config/add-ons/commentatic.yaml

Then field the following parameters :

- `username_field`: Sets the field name containing the username (default: `username`)
- `email_field`: Sets the field name containing the email (default: email)
- `comment_field`: Sets the field name containing the comment (default: `comment`)
- `comment_per_page`: Number of comments per page
- `comment_folder`: Set the folder that will host your comments and located in the `_content` folder of your Statamic installation. Use a underscore before the folder name in order to hide the navigation (default: `_comments`)

Create a folder dedicated to the comments in the `_content` folder. Assign write permissions to the folder.

## Comment listing usage

Use the `{{commentatic:listing}}` method in the same way as for the Statamic `{{entries:listing}}` method. The options are similar except that you don't have to specify the limit (defined in the config file).

    {{ commentatic:listing }}
    <div class="row">
        <div class="entry clearfix">
          {{ if no_results }}
            <h3>Hey! No comment. <a href="#post-comment">Why not post one ?</a></h3>
          {{ else }}
          <div class="span2">
            <div class="entry-meta">{{ username }}</div>
            <a href="{{ gravatar_profile }}">{{ avatar }}</a>
            <div class="entry-meta">Posted on <span class="date"> {{ last_modified format="F jS, Y H:i" }}</span>.</div>
          </div>
          <div class="span6">
                  {{ content }}
            </div>
        </div>
        {{ endif }}
    </div>
    {{ /commentatic:listing }}

If you use the pagination, The use is similar to the method `{{ entries:pagination }}`. Except that you don't specify the limit parameter (defined in the config file).

## Comment form usage
    
To display the form for writing a comment, use the method `{{ commentatic:form }}`

Here are the options :

`class` : Sets the class name of the form tag
`id` : Sets the ID of the form tag
`required` : Sets the required fields. Separated by |.(no default but username, email and comment are always required)
`honeypot` : activate or not the honeypot against bot (default : true)

Example of usage :

  {{ commentatic:form honeypot="true" id="post-comment" }}
      {{ if error}}
          <h1>Error</h1>
          <ul>
          {{ errors }}
              <li>{{error}}</li>
          {{ /errors }}
          </ul>
      {{ endif }}
  
      {{ if success }}
          <p>Your message has been sent!</p>
      {{ else }}
          {{ if logged_in }}
          <div class="clearfix">
            <p class="offset2">Logged in as <strong>{{ username }}</strong></p>
          </div>
          {{ else }}
          <p class="offset2">
              <label for="username">Username*:</label><br />
              <input type="text" name="username" id="username" value="{{ post:username }}" placeholder="Your Username">
          </p>
          <p class="offset2">
              <label  for="from">Email*:</label><br />
              <input type="text" name="email" id="email" value="{{ post:email }}" placeholder="Your email address">
          </p>
          {{ endif }}
          <p class="clearfix">
            <label class="span2" for="comment">Your comment:</label>
            <textarea class="span6" type="text" rows="5" name="comment" id="comment" placeholder="Your thoughts here !">{{ post:comment }}</textarea>
          </p>
          <p class="clearfix"><input class="offset2 btn" value="Send comment" type="submit"></p>
      {{ endif }}
  {{ /commentatic:form }}

## Helpers

- `{{ commentatic:comment_count }}`: returns the number of comments of an folder
- `{{ commentatic:comment_count folder="my_folder"}}`: returns the number of comments for a specific folder
- `{{avatar}}`: print the user's gravatar if available
- `{{gravatar_profile}}`: print the user's gravatar profile link if available

## TODO

- Better management publishing date and publishing time
- Better memory of users who have already posted (Cookie)
- Social login (Twitter, Facebook, etc...)
- Performance improvement (Something like caching)
- More tests!
- Code cleanup