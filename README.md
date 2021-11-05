# Event App

The [front-end][1] provides a single-page application to visualize the details of an event and set a check back reminder.

## Requirements

- [npm][2]
- [http-server][3] (only required to start the production version of the front-end)

## Setup

### 1. front-end setup

#### 1.1 Switch into the front-end directory

```sh
cd front-end
```

#### 1.2 Install NPM packages

```sh
npm install
```

#### 1.3 Create your `.env` file

See file [.env.example][4] for an example.

#### 1.4 Compiles and hot-reloads for development

```sh
npm run serve
```

#### 1.5 Compiles and minifies for production

```sh
npm run build
```

#### 1.6 Starts production

```sh
npm run serve:production
```

<!-- MARKDOWN LINKS -->

[1]: ./front-end
[2]: https://nodejs.org/en/download/current/
[3]: https://www.npmjs.com/package/http-server
[4]: ./front-end/.env.example
