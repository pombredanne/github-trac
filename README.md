Github Trac Hook
================

I have officially taken this project over, so all forks of this project should happen here not upstream.

I took this project over serveral years back, but have since moved away from using Trac.

I'll try to merge pull requests when I can, but I have ZERO tests for this module, use it with caution.

_Anyone that wants to submit tests please do_

Issues
------

Please file bug reports here:
https://github.com/davglass/github-trac/issues


About
-----

This trac plugin is designed to accept a GitHub Post-Receive url.

It also allows you to replace the builtin Trac browser with redirects to the GitHub source browser.

More information about the Post-Receive Hook can be found here:
http://github.com/guides/post-receive-hooks

To install this Trac Plugin:

    1. Clone the repository:
        git clone git://github.com/davglass/github-trac.git

    2. Install SimpleJSON:
        easy_install simplejson

    3. Install the Plugin:
        cd github
        sudo python setup.py install

    4. Configure Trac by editing your trac.ini
        
        [components]
        github.* = enabled

        [github]
        apitoken = YOUR GITHUB API TOKEN
        closestatus = closed (This is optional, defaults to closed
        browser = http://github.com/davglass/tree/master

        #Optional - perform a fetch on the local git repository
        autofetch = true

        #Optional - restrict commit hook to branches, comma separated. Default is special value 'all', meaning no restriction.
        branches = production,master,stable,dev

        #Optional - this will be appended to your commit message and used as trac comment
        # Following keys are available: 'commit' and everything from github payload.
        # See https://help.github.com/articles/post-receive-hooks for details of payload. See source for others.
        comment_template = Changeset: {commit[id]}
        
    5. Go to the Admin page for your project on GitHub. Then select the services tab.
        Under the: Post-Receive URLs
        Place a link to your Trac repository, in a format like this:
        
        http://yourdomian.com/projects/projectname/github/APITOKEN

        You can get your GitHub API token from your accounts page.
        This is for your security, only those that know the API Token will able to post to this url

        You can override branches configuration from trac.ini via query parameter in trac URL:
        http://yourdomian.com/projects/projectname/github/APITOKEN?branches=production,staging

    6. All done.


Hook
----

The commit hook is designed to close or mark tickets that are attached to a commit message.


It searches commit messages for text in the form of:
    command #1
    command #1, #2
    command #1 & #2 
    command #1 and #2

Instead of the short-hand syntax "#1", "ticket:1" can be used as well, e.g.:
    command ticket:1
    command ticket:1, ticket:2
    command ticket:1 & ticket:2 
    command ticket:1 and ticket:2


The Code Browser
----------------

The code browser portion of the plugin is designed to replace the code browser built into Trac with a
redirect to the GitHub source browser.

In order for this to work properly, you still need to have Trac set to show a git repository. Turning this off
will not show source control links in other places (timeline). This git repository needs to be a synced clone of
your GitHub repository. Trac requires the GitPlugin in order to show a Git Repository: http://trac-hacks.org/wiki/GitPlugin


This plugin will attempt to intercept the `/browser` and `/changeset` url's and redirect you to the proper GitHub url.
