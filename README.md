This project has moved. Please report issues and submit pull requests here instead:
https://github.com/git/git-scm.com
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
# [GIT-SCM](https://github.com/git/git-scm.com)
This is the web application for the git-scm.com site. 
It is meant to be the first place a person new to Git will
land and download or learn about the Git SCM system.

This app is written in `Ruby on Rails` and deployed on `Heroku`


# Setup
You'll need a Ruby environment to run `Rails`. First do:

`$ rvm use 
$ bundle install`

Then you need to create the database structure:
`$ rake db:migrate`

Alternatively you can run the script at script/bootstrap which will set up Ruby dependencies and the local SQLite database.

Now you'll want to populate the man pages. You can do so from a local Git source clone like this:

`$ GIT_REPO=../git/.git rake local_index`

This will populate the man pages for all Git versions.
You can also populate them only for a specific Git version faster

`$ version=v2.23.0
$ GIT_REPO=../git/.git REBUILD_DOC=$version rake local_index`

Or you can populate the man pages from GitHub 
much slower like this:

`$ export GITHUB_API_TOKEN=github_personal_auth_token
$ rake preindex  # all versions
$ REBUILD_DOC=$version rake preindex  # specific version`

Similarly, you can also populate the localized man pages.
[From a local clone of](https://github.com/jnavila/git-html-l10n)

`$ GIT_REPO=../git-html-l10n/.git rake local_index_l10n  # all versions`
`$ GIT_REPO=../git-html-l10n/.git REBUILD_DOC=$version rake local_index_l10n  # specific version`
Or you can do it from GitHub (much slower) like this:

`$ export GITHUB_API_TOKEN=github_personal_auth_token`
`$ rake preindex_l10n  # all versions`
`$ REBUILD_DOC=$version rake preindex_l10n  # specific version`
Now you need to get the latest downloads for the downloads pages:
`$ rake downloads`
Now you'll probably want some book data. 
You will have to have access to the Pro Git project on GitHub through the API.

`$ export GITHUB_API_TOKEN=github_personal_auth_token`
`$ rake remote_genbook2`
If you have 2FA enabled, you will need to create a Personal Access Token.
That will generate the book content from the Asciidoc files fetched
from the online repository
and post it to the Rails server database.
You can select a specific language by
indicating it in the `GENLANG` environment variable:

`$ GENLANG=zh rake remote_genbook2`
Alternatively, you can get the book content
from a repository on your computer by specifying the
path in the `GENPATH` environment variable to the local_genbook2 target:

`$ GENLANG=fr GENPATH=../progit2-fr rake local_genbook2
Now you can run the Rails site to take a look.

$ ./script/server`
The site should be running on: <http://localhost:5000>

Testing
To run the tests for this project,
run:

`$ rspec`
To run the website for testing purposes
run:

`$ ./script/server`

# Contributing
If you wish to contribute to this website, please fork it on GitHub,
push your change to a named branch, then send a pull 
request. If it is a big feature, you might want to start 
an issue first to make sure it's something that will be 
accepted. If it involves code, please also write tests for it.

# Adding new GUI
The list of GUI clients has been constructed by the community for a long time.
If you want to add another tool you'll need to follow a few steps:
Add the GUI client details at the
YAML file: <https://github.com/git/git-scm.com/blob/main/resources/guis.yml>

The fields name, url, 
price, license should be very straightforward 
to fill.
The field image_tag corresponds
to the file 
name of the image of
the tool 
**without path, just the filename** 
platforms is a list of at least [1](one)
platform in which the tool is supported. 
The possibilities are: 
|--|
Windows
Mac
Linux
Android
iOS

order can be filled with
the biggest number already existing, plus 1 
Adding to the bottom - this will be covered
in the following steps
trend_name is an 
**optional**
field that can be used for helping sorting the clients
also covered in the next steps
[Add the image to public](/ images/guis/<GUI_CLIENT_NAME>@2x.png and public/images/guis/<GUI_CLIENT_NAME>.png>)
making sure the aspect ratio matches a `588:332 image`

# Sort the tools
From the root of the repository, 
run: `$ ./script/sort-gui`
A list of google trends url's will be displayed at 
the bottom if everything went well.
Open each and
check if the clients are sorted.
If the clients are not 
sorted, just fix the order 
by changing the order field, 
bubbling the more 'known' 
clients all the way up.
Repeat until the order stabilizes.
It is possible that
your new GUI client doesn't have good results in Google Trends. 
You can try similar terms 
for instance, adding the git keyword sometime helps
If you find any similar term that returns better results, 
add the trend_name field to the GUI client.
Have a look at the Tower and Cycligent Git Tool
tools example.
The script makes some basic verifications.
If there was some problem,
it should be easily visible in the output
If you have more than 1 tool with the same name a 
::warning::
will appear: **======= WARNING: THERE ARE DUPLICATED GUIS =======**
If you are using the same order value for more than 1 tool,
a ::warning:: will appear: **======= WARNING: THERE ARE DUPLICATED ORDERS (value: <VALUE>) =======**

# FAQ
While setting the repo if you find any error, check
if it's a known issue and the corresponding solution below.
An error occurred while installing pg (1.2.3),[0]
and Bundler cannot continue. If you got this error
when running `bundler install`
then you need to install `postgresql` on your OS. 
Check this stackoverflow topic 
for more details.<https://www.stackoverflow.com>

# License
The source code for the site is licensed under the
[AGPL 3 license, which you can find in](https://github.com/Apetree100122/gitscm-old/blob/master/LICENSE%20AGPL%203)
