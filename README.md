# README

This is the source code for https://dpfrakes.dev.

- Hosted on [GitHub](https://github.com/dpfrakes/dpfrakes-site)
- Built with [Hugo](https://gohugo.io)
- CMS by [Forestry.io](https://app.forestry.io/sites)
- Forms hosted by [Formspree](https://formspree.io/forms)
- Media hosted by [AWS](https://console.aws.amazon.com/s3/buckets/dpfrakes)

### Requirements

```sh
# Install hugo
brew install hugo
```

### Setup

#### Develop

```sh
# Start server
hugo server

# Create new post
hugo new posts/new-post.md

# Build
hugo
```

#### Deploy

To deploy content changes, log in to [Forestry.io](https://app.forestry.io/sites) and publish changes there.

To deploy code changes, or to manually change and deploy content, push changes to `master` branch.

If the deployment GitHub Action is not working, build the project with `hugo`, then run the following to deploy from CLI:

```sh
# Install Firebase
npm i -g firebase-tools

# Log in to Firebase
firebase login

# Deploy
firebase deploy
```
