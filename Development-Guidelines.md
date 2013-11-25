# Guidelines for Development

## Spelling

An initial spell check of any new document in the text editor **must** be made before committing that file to the repository. When making changes to an existing HTML template or file of any sort with hard coded strings, a spell check of the changed file(s) **must** be completed before committing.

## Links

Avoid making commits to a project with `href` attributes of `#` or null `href` attributes unless specifically approved by the Interactive Producer.

Make links absolute (beginning with '/') unless specifically instructed otherwise. This ensures portability between domains and doesn't tie the link to the page's location.

Before a site can go live, the Interactive Producer will verify that all hyperlinks are properly linked.

## Lorem Ipsum and Filler Content

Avoid making commits to a project with Lorem ipsum or other filler text. 

Any filler content committed to a project **must** be prefixed with `FPO >> ` and appended with `<< END FPO`.

Any FPO images committed to a project **must** have the letters `FPO` over the top of the image in a manner visible to normal viewing of that image.

Additionally, any FPO images committed to a project **must** have a filename prefix of `fpo-`.

## Development 

The Interactive Producer will be doing a code review of any code you create. Framiliarize yourself with [the go-live checklist](https://github.com/jbergantine/django-newproj-template/wiki/Go-Live-Checklist).

## Git

Git workflow follows the [Feature Branch Workflow](http://www.atlassian.com/git/workflows#!workflow-feature-branch). 

## Feature Branches

A feature branch is a way to work on something in an isolated way without breaking the main line branch but while still working with other people so they can see the code you're writing or the assets you're creating.

Whatever you are doing you will always create a new feature branch to do it. The new branch should follow the feature branch naming convention: *feature-X* where *X* is a short description of the new feature being implemented or *issue-X* where *X* is the ticket number of the ticket being addressed in the feature branch.

### Assigning and Fixing an Issue / Creating a New Feature Branch

When a issue ticket is assigned, follow this procedure. If you are working on a feature branch without a ticket follow this same procedure except don't worry about referencing the ticket number.

1. Create and checkout a new feature branch off of the main line branch if necessary or create a sub-feature branch off of the feature branch as indicated. 

        $ git checkout -b <new-branch-name> <parent-branch-name>

1. Make changes on that branch and optionally commit with a message that includes the text "addresses #<issue-number>". You may also amend this commit message if the last commit didn't include the message; before pushing (or even after pushing if you push again) you can run: `$ git commit --amend -m "addresses #<issue-number>"`).
1. Push. Use the `-u` flag on the first push of a new branch.

        $ git push -u origin <new-branch-name>

1. Login to the BitBucket control panel and create a pull request.
    1. Choose your feature branch as the source.
    1. Choose the parent that you branched off of as the destination.
    1. Describe what your feature branch does in the pull request message. List any issues the feature branch addresses. If you're just issuing a pull request to review a work in progress and not close a feature or issue, mention that.
    1. Tag the Interactive Producer and whoever else needs to see the feature (perhaps the Creative Director) in the Reviewers section. (Note the `@` sign is taken literally in this field so omit it.)
    1. Click the button to close the branch after the pull request is merged if the feature branch can be considered done after the merge (if you're just issuing a pull request to have a look at a work in progress do not tick this button).

### Reviewing a Pull Request
    
Once assigned a pull request is assigned for review, the reviewer will:

1. Look at the change set on BitBucket and pull locally to test. Look at any issue numbers referenced in the pull request.

#### If the change set is on a branch you already have:

1. Checkout the branch if you aren't already working on it:

        $ git checkout <branch-name>

1. Check for incoming changes:

        $ git fetch origin && git log ..origin/<branch-name>

1. Check for outgoing changes:

        $ git fetch && git log origin/<branch-name>..

1. Review in detached HEAD state:

        $ git checkout origin/<branch-name>

1. `runserver` and manually review.
1. Append a commit message to close any open tickets: "resolves #<issue-number>":

        $ git commit --amend -m "resolves #<issue-number>"

1. Branch into a new branch prefixed with the name `review-`:

        $ git checkout -b review-<branch-name>

1. Merge the branch onto the appropriate deployment- or feature-branch before deploying:
   
        $ git checkout <branch-branch>
        $ git merge review-<branch-name>
        $ git push origin <branch-name>

1. Cleanup:

        $ git br -d <branch-name>


#### If the change set is to a new (to you) branch:

1. Pull down and demo the fix:

        $ git fetch origin
        $ git checkout <branch-name>

1. `runserver` and manually review.
1. Append a commit message to close any open tickets: "resolves #<issue-number>":

        $ git commit --amend -m "resolves #<issue-number>"

1. Merge the branch onto the parent branch (this will automatically approve and close the pull request):

        $ git checkout <parent-branch>
        $ git merge <branch-name>
        $ git push origin <parent-branch>

1. Cleanup:

        $ git br -d <branch-name>