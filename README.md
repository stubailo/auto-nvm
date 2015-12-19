# auto-nvm

Automatically run the right version of Node and NPM for each project

### Background

[Meteor](https://www.meteor.com/) does version management for you, and automatically installs/runs the right versions of everything based on the `.meteor/release` file that exists in every Meteor project. As a result, you can basically always just `git clone` a Meteor project, and type `meteor` in the root directory to run it, without worrying about versions of Meteor, Node, NPM, etc.

I'd like to have the same experience for Node - it would be great to just download any project, run one command, and be sure that it's going to run the way the author intended. Many projects now need to document in the README which versions of Node they expect, but there's already a field for that in `package.json` - the `engines` field!

Could we use this field to just run the right version of Node and NPM based on the project we're in?

### Desired UX

```
git clone https://github.com/webpack/react-starter
auto-nvm npm install
auto-nvm npm start
```

This should first install the versions of Node and NPM recommended by the project's `package.json` file, and then run the right commands.

`auto-nvm node` and `auto-nvm npm` should be reasonable things to alias to `node` and `npm` respectively; in the ideal universe where each project specifies its `node` and `npm` versions correctly, you should never have to manually decide which version to run again!

#### Creating a new project

When using `auto-nvm npm init`, the resulting `package.json` should acquire an `engines` field which specifies the newest stable versions of Node and NPM by default.

#### Error handling

There are certain conditions where `auto-nvm` will encounter issues:

1. `engines` is not specified in `package.json` - it should attempt to fall back to NVM's default versions for the command. You should be able to use `nvm use` as usual.
2. `engines` is specified, but not in a valid format. In this case it might make sense to print a warning.
3. `engines` is specified, but is not a version that NVM can install. In this case it might make sense to print a warning.
