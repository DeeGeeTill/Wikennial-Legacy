
<p align="center">
  <h1>Wikennial</h1>
</p>
<p align="left">
  <i>A nice little team wiki for people to organise their thoughts. Also known as Outline</i>
  <br/>
  <img src="https://media.discordapp.net/attachments/686001040486563948/688701861770821632/screenzy-1584269709067.png" alt="Outline" width="800" />
</p>
<p align="center">
  <a href="https://circleci.com/gh/outline/outline" rel="nofollow"><img src="https://circleci.com/gh/outline/outline.svg?style=shield&amp;circle-token=c0c4c2f39990e277385d5c1ae96169c409eb887a"></a>
  <a href="https://github.com/prettier/prettier"><img src="https://img.shields.io/badge/code_style-prettier-ff69b4.svg?style=flat"></a>
  <a href="https://github.com/styled-components/styled-components"><img src="https://img.shields.io/badge/style-%F0%9F%92%85%20styled--components-orange.svg"></a>
</p>

This is the source code that, at one point, ran **Wikennial/Outline** and all the associated services. If you'd like to run your own copy of Wikennial or contribute to development then this is the place for you.

**Wikennial** is no longer under development as a result of data destruction, however, this source code has been released as a gesture of goodwill.

## Dependency Overview

Wikennial requires the following dependencies:

- Node.js >= 14
- Postgres >=9.5
- Redis >= 4
- AWS S3 storage bucket for media and other attachments
- Slack or Google developer application for authentication


### Development
This guide assumes a [WiiToo!](https://wiibrew.org/wiki/WiiToo!) environment on a Nintendo Wii. As a result, I cannot guarantee that this guide will work for all Linux/Windows Distros, but you're more than welcome to submit an issue.

In development you can quickly get an environment running using Docker by following these steps:

1. Install these dependencies if you don't already have them
  1. [Docker for Desktop](https://www.docker.com) (HYPER-V)
  2. [Node.js](https://nodejs.org/) (v12 LTS preferred)
  3. [Yarn](https://yarnpkg.com)
  4. [Nginx](https://nginx.com)
1. Clone this repo
1. Register a Slack app at https://api.slack.com/apps
1. Copy the file `.env.sample` to `.env`
1. Fill out the following fields:
    1. `SECRET_KEY` (follow instructions in the comments at the top of `.env`)
    1. `SLACK_KEY` (this is called "Client ID" in Slack admin)
    1. `SLACK_SECRET` (this is called "Client Secret" in Slack admin)
1. Configure your Slack app's Oauth & Permissions settings 
    1. Add `http://localhost:3000/auth/slack.callback` as an Oauth redirect URL
    1. Ensure that the bot token scope contains at least `users:read`
1. Run `make up`. This will download dependencies, build and launch a development version of Wikennial.


## Development

### Server

Wikennial uses [debug](https://www.npmjs.com/package/debug). To enable debugging output, the following categories are available:

```
DEBUG=sql,cache,presenters,events,logistics,emails,mailer
```

## Migrations

Sequelize is used to create and run migrations, for example:

```
yarn sequelize migration:generate --name my-migration
yarn sequelize db:migrate
```

Or to run migrations on test database:

```
yarn sequelize db:migrate --env test
```

## Structure

Wikennial is composed of separate backend and frontend application which are both driven by the same Node process. As both are written in Javascript, they share some code but are mostly separate. We utilize the latest language features, including `async`/`await`, and [Flow](https://flow.org/) typing. Prettier and ESLint are enforced by CI.

### Frontend

Wikennial's frontend is a React application compiled with [Webpack](https://webpack.js.org/). It uses [Mobx](https://mobx.js.org/) for state management and [Styled Components](https://www.styled-components.com/) for component styles. Unless global, state logic and styles are always co-located with React components together with their subcomponents to make the component tree easier to manage.

The editor itself is built on [Prosemirror](https://github.com/prosemirror).

- `app/` - Frontend React application
- `app/scenes` - Full page views
- `app/components` - Reusable React components
- `app/stores` - Global state stores
- `app/models` - State models
- `app/types` - Flow types for non-models

### Backend

Backend is driven by [Koa](http://koajs.com/) (API, web server), [Sequelize](http://docs.sequelizejs.com/) (database) and React for public pages and emails.

- `server/api` - API endpoints
- `server/commands` - Domain logic, currently being refactored from /models
- `server/emails`  - React rendered email templates
- `server/models` - Database models
- `server/policies` - Authorization logic
- `server/presenters` - API responses for database models
- `server/test` - Test helps and support
- `server/utils` - Utility methods
- `shared` - Code shared between frontend and backend applications

## Tests

I aim to have sufficient test coverage for critical parts of the application and aren't aiming for 100% unit test coverage. All API endpoints and anything authentication related should be thoroughly tested.

To add new tests, write your tests with [Jest](https://facebook.github.io/jest/) and add a file with `.test.js` extension next to the tested code.

```shell
# To run all tests
yarn test

# To run backend tests
yarn test:server

# To run frontend tests
yarn test:app
```

## Contributing

Wikennial is no longer being worked on, so basically, fair game lmao.

## License

Wikennial is under the MIT License.
