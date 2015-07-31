# generator-ramlapi

`generator-ramlapi` is a generator for [Yeoman](http://yeoman.io) that generates a project scaffold for API development in RAML and JSON Schema.

RAMLAPI uses conventions to support files that are added without needing to modify the Gruntfile.js that runs the validation and document production.

*Features*

* RAML validation
* RAML HTML document generation (via RAML2HTML) with improved HTML templates
* JSON Schema validation
* Example validation against JSON Schema

## Getting Started:

*Installing Dependencies*

Install [NodeJS](https://nodejs.org/download/). On OSX, installation via [NVM](https://www.npmjs.com/package/nvm) and/or [Homebrew](brew.sh) is highly recommended.

Update [NPM](https://www.npmjs.com). Depending on your installation, you may need to use `sudo` for the following commands.

```bash
npm install -g npm
```

Install Yeoman, [Grunt](gruntjs.com) and generator-ramlapi:

```bash
npm install -g yo grunt-cli generator-ramlapi
```

## Beginning a project

Create a directory for your RAML API project and `cd` into it.

Run the generator like this:

```bash
yo ramlapi
```

The generator will ask you a number of questions. Answer these as completely as you can, but all can be changed later if need be.

After the questions, the generator will install the NPM dependencies for your project.

## Usage

To run the validation and build HTML docs, simply run the command:

```bash
grunt
```

Validates the JSON Schema, RAML, and produces documentation in the `public/` directory.

### Project Structure

* *`<%= projectTitle %>-<%= apiVersion%>.raml`*: is located in the root directory and is expected to be named with the `name` and `version` values from the `package.json` file.
* *`raml/`*: RAML fragment files referenced by the primary RAML.
* *`schema/`*: JSON Schema files referenced in RAML files (or fragments).
* *`examples/`*: JSON example files for the JSON Schema.
* *`public/`*: a folder suitable to publish to a GitHub project page with generated documentation.
* *`templates/`*: contains the RAML2HTML templates that are used instead of the default templates.

### RAML Notes

RAML files should not include JSON Schema directly, but should instead use `!include` to include them from the `schema/` directory.

[raml2html](https://github.com/kevinrenskers/raml2html) is used to generate the documentation HTML. Since it currently doesn't support references in the JSON Schema, this project does the following:

For each schema referenced in the RAML, this generator produces a `\<jsonschemafile\>-full.json` file which contains the content from `jsonschemafile.json` and the content of the external references to other JSON Schema files added under a `definitions` schema.

Do not write JSON Schema directly into your RAML files unless it contains no `"$ref"` references to other schema.

### Schema Notes

Schema files are expected to be stored in the `schema/` directory.

The concatenation of the JSON Schema is fairly naive at present. It has the following expectations:

1. Schema that is referenced in a RAML file may contain a top level object.
2. All JSON Schema files are expected to define a top level `id`.
3. JSON Schema that is referenced via a `"$ref"` is expected to be defined under a `"definitions"` sub-schema. See `schema/date-time.json` for an example.
4. There are no namespace checks done, so watch out for name collisions.

Example files are stored in the `examples/` directory. Examples must begin with the base name of the schema file they belong to. For example, each of the following examples would be validated against `schema/sample.json`:

1. `examples/sample.json`
2. `examples/sample2.json`
3. `examples/sample-3.json`

## Generator improvements to come

1. Consider moving to Gulp ;-)
2. scaffold the github page

## License

Apache-2.0
