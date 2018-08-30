## Updated

**This fork is intended to implement the PR [here](https://github.com/hubot-scripts/hubot-auth/pull/32) for Hubot 3.x**

My version of Hubot is a frail, ephemeral thing, with almost nothing in the way of persistence. I would _much_ prefer to store roles and the user list in GitHub repo rather than in Hubot's brain.

[![Build Status](https://travis-ci.org/hubot-scripts/hubot-auth.svg?branch=master)](https://travis-ci.org/hubot-scripts/hubot-auth)

Assign roles to users and restrict command access in other scripts.

See [`src/auth.coffee`](src/auth.coffee) for full documentation.

# Hubot: hubot-auth

## Installation

Add **hubot-auth** to your `package.json` file:

```
npm install --save hubot-auth
```

Add **hubot-auth** to your `external-scripts.json`:

```json
["hubot-auth"]
```

Run `npm install`

## Sample Interaction

```
user1>> hubot user2 has jester role
hubot>> OK, user2 has the jester role.
```

## Sample Usage
### Restricting commands
```coffee
module.exports = (robot) ->
  # Command listener
  robot.respond /some command/i, (msg) ->
    role = 'some-role'
    user = robot.brain.userForName(msg.message.user.name)
    return msg.reply "#{name} does not exist" unless user?
    unless robot.auth.hasRole(user, role)
      msg.reply "Access Denied. You need role #{role} to perform this action."
      return
    # Some commandy stuff
    msg.reply 'Command done!'
```
### Example Interaction
```
user2>> hubot some command
hubot>> Access Denied. You need role some-role to perform this action.
user1>> hubot user2 has some-role role
hubot>> OK, user2 has the some-role role.
user2>> hubot some command
hubot>> Command done!
```
