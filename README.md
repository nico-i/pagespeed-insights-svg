# PageSpeed Insights SVG

<br/>

![Example Insights for https://inico.dev/](docs/img/inico-insights.svg)

[![npm](https://nodei.co/npm/psi-svg.png?downloads=true)](https://www.npmjs.com/package/psi-svg)

This node module performs a [Google PageSpeed Insights analysis](https://developers.google.com/speed/pagespeed/insights/) on a given webpage and returns the result as an SVG.

## Installation

### npm

```bash
npm i -g psi-svg
```

[More info on `npm`](https://www.npmjs.com/)

### bun

```bash
bun i -g psi-svg
```

[More info on `bun`](https://bun.sh/)

### yarn

```bash
yarn global add psi-svg
```

[More info on `yarn`](https://yarnpkg.com/)

## Usage

The module can be used as a CLI tool or as a web server. Different options can be used to customize the output for each use case.

### CLI

To use the module as a CLI tool, simply run it with the URL of the webpage to analyze and the path to the output SVG.

Example:

```bash
psi-svg https://www.example.com ./out.svg
```

#### Flags

The following flags can be used to customize the analysis:

| Flag                   | Description                                       | Type                                                                   | Values                                                 |
| ---------------------- | ------------------------------------------------- | ---------------------------------------------------------------------- | ------------------------------------------------------ |
| `--strategy` or `-s`   | The strategy to use for the analysis.             | `desktop` \| `mobile`                                                  | `mobile`                                               |
| `--categories` or `-c` | The categories to include in the analysis as CSV. | `performance` \| `accessibility` \| `best-practices` \| `seo` \| `pwa` | `performance, accessibility, best-practices, seo, pwa` |
| `--legend` or `-l`     | Whether to include the legend in the SVG.         | `true` \| `false`                                                      | `true`                                                 |

#### npx

The module can also be run without installing it by using `npx`:

```bash
npx psi-svg https://www.example.com ./out.svg
```

### Server

To run the application as a web server, the `--srv` or `-s` flag can be used:

```bash
psi-svg -s
```

By default the server will listen on port 3000. To change this, the `--port` or `-p` option can also be set:

```bash
psi-svg -s -p 8080
```

After the server has been started, an insights SVG can be generated by sending a GET request to the server with the URL to analyze as a query parameter:

```bash
curl http://localhost:3000/?url=https://www.example.com > example-insights.svg
```

#### Query Parameters

The server accepts similar parameters as the CLI tool:

| Parameter    | Description                                       | Values                                                                 | Default                                                |
| ------------ | ------------------------------------------------- | ---------------------------------------------------------------------- | ------------------------------------------------------ |
| `url`        | The URL to analyze.                               | `string`                                                               | -                                                      |
| `strategy`   | The strategy to use for the analysis.             | `desktop` \| `mobile`                                                  | `mobile`                                               |
| `categories` | The categories to include in the analysis as CSV. | `performance` \| `accessibility` \| `best-practices` \| `seo` \| `pwa` | `performance, accessibility, best-practices, seo, pwa` |
| `legend`     | Whether to include the legend in the SVG.         | `true` \| `false`                                                      | `true`                                                 |

This GET request for example

```bash
curl http://localhost:3000/?url=https://www.example.com\&strategy=desktop\&categories=performance,accessibility,seo\&legend=false > example-insights.svg
```

will return this SVG

![Example Insights for https://www.example.com](./docs/img/example-insights.svg)

*Note: this analysis ran on the 17. November of 2023; the example page may be prone to change and the results may not be accurate anymore.*

### Environment Variables

The following environment variables can be used to customize the application:

| Variable           | Description                                                                                                     | Values   | Default |
| ------------------ | --------------------------------------------------------------------------------------------------------------- | -------- | ------- |
| `DOMAIN_WHITELIST` | A comma-separated list of domains to allow requests from. Currently only supports domain name i.e. no wildcards | `string` | -       |

Setting the `DOMAIN_WHITELIST` environment variable to `example` for example will allow requests from `https://example.com` and `https://example.org`. This is especially useful when running the web server in a public environment.

## Docker

The application can also be run as a Docker container by following the following steps, although this requires [docker to be installed](https://docs.docker.com/engine/install/).

1. Clone the repository

```bash
git clone https://www.github.com/nico-i/psi-svg
```

2. Build the image

```bash
docker build -t psi-svg .
```

3. Run the image

```bash
docker run -p 3000:3000 psi-svg
```

## Github Actions

The application can also be run as a Github Action an example workflow can be found in [`.github/workflows/pagespeed.yml`](.github/workflows/pagespeed.yml). The results of which are visible in [`docs/img/`](docs/img/). To use the action, simply copy the workflow file to your repository and modify the value of the `URL_TO_ANALYZE` and `RESULTS_DIR` variables. Also ensure that the Github Action Workers have write access to the repository. You can configure this under `Settings > Code and automation > Actions > General > Workflow permissions`.

## Credits

This project is based on [ankurparihar](https://github.com/ankurparihar)'s [readme-pagespeed-insights](https://github.com/ankurparihar/readme-pagespeed-insights).

This project improves the original project in the following ways:

* Added a CLI implementation
* Added Docker compatibility
* Removed unnecessary options which previously bloated the code
* Better SVG generation with individual SVG-files instead of inline SVG strings
* SVG styling CSS moved to an individual file to facilitate intellisense and linting
* Improved the file structure with Domain Driven Design (DDD)
* Added the option to disable the legend
* *TODO: Added automated tests*

## License

See [LICENSE](LICENSE).
