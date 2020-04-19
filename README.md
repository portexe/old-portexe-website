## To run: 

``` bundle exec jekyll serve ```

## To change the social media icons:

Simply go to ```config.yml``` and comment out the username for the platform you want to remove.

## Thumbnails

The main images used to represent blog posts are 760x400

## Notes:

- To update Gems I just delete the gemfile and then run:

  ```bundle install```

- I need to automate as much as I can. Things that I can automate are:
    1. Thumbnail cropping and uploading
        - Node script that crops the image and then uploads it to the hosting service.
        - AWS for hosting images? Or another service.
    2. Adding a post to the repo and then deploying
        - Simple Node script that will let me paste in the post, and then it adds the post to the local repo and pushes it to GitHub all in one go.