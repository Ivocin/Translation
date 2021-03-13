> * 原文地址：[How to build a CLI with Node.js](https://www.twilio.com/blog/how-to-build-a-cli-with-node-js)
> * 原文作者：[DOMINIK KUNDEL](https://www.twilio.com/blog/author/dkundel)
> * 本文永久链接：[https://github.com/Ivocin/Translation/Blogs/how-to-build-a-cli-with-node-js.md](https://github.com/Ivocin/Translation/Blogs/how-to-build-a-cli-with-node-js.md)
> * 译者：[Ivocin](https://github.com/Ivocin/)
> * 校对者：

# 如何使用 Node.js 构建 CLI

![](https://s3.amazonaws.com/com.twilio.prod.twilio-docs/images/atZ3n9vMFjjXDl_XxDtL_FCRSOt6EF0d8LnbMRCCJQUesM.width-808.png)

Node.js 中内置的命令行界面（CLI）能够让你充分利用庞大的 Node.js 生态系统自动化执行重复性任务。感谢像 [`npm`](https://www.npmjs.com/) 和 [`yarn`](https://yarnpkg.com/) 这样的包管理器，它们可以在多个平台上轻松分发和使用。在本文中，我们将介绍为什么要编写 CLI、如何使用Node.js 来创建 CLI、一些实用的包以及如何发布你的新 CLI。

## 为什么使用 Node.js 创建 CLI

Node.js 如此受欢迎的原因之一是其丰富的包生态系统，在 [`npm` 注册表](https://npmjs.com)中有超过 900,000 个包。通过使用 Node.js 编写 CLI，你可以利用这个生态系统，包括大量针对 CLI 的软件包。 其中：

*   [`inquirer`](http://npm.im/inquirer)、 [`enquirer`](http://npm.im/enquirer)  或者 `[prompts](https://npm.im/prompts)` ：复杂的输入提示
*   [`email-prompt`](http://npm.im/email-prompt) ：简化电子邮件输入提示
*   [`chalk`](http://npm.im/chalk) or `[kleur](https://npm.im/kleur)` ：彩色输出
*   [`ora`](http://npm.im/ora)：漂亮的加载状态动画
*   [`boxen`](http://npm.im/boxen)：在输出周围绘制边框
*   [`stmux`](http://npm.im/stmux)：提供 `tmux`(Terminal Multiplexing) 风格的 UI
*   [`listr`](http://npm.im/listr)：进度列表
*   [`ink`](http://npm.im/ink)：使用 React 构建 CLI
*   [`meow`](http://npm.im/meow) 或者 [`arg`](http://npm.im/arg)：基础参数解析
*   [ `commander`](http://npm.im/commander) 和 [`yargs`](https://www.npmjs.com/package/yargs)：复杂参数解析以及子命令支持
*   [`oclif`](https://oclif.io/) 由 Heroku 构建的可扩展 CLI 框架 (也可以选择 `[gluegun](https://infinitered.github.io/gluegun/#/)`)

Additionally there are many convenient ways to consume CLIs published to `npm` from both `yarn` and `npm`. Take as an example `create-flex-plugin`, a CLI that you can use to bootstrap a plugin for [Twilio Flex](https://twilio.com/flex). You can install it as a global command:
此外，`yarn` 和 `npm` 都提供了许多方便的方法来使用发布到 `npm` 的 CLI。我们以 `create-flex-plugin` 为例，使用它可以创建 [Twilio Flex](https://twilio.com/flex) 插件。你可以将其安装为全局命令：

```
# 使用 npm:
npm install -g create-flex-plugin
# 使用 yarn:
yarn global add create-flex-plugin
# 安装成功后你就可以使用了：
create-flex-plugin
```

也可以将其作为项目的特定依赖：

```
# 使用 npm:
npm install create-flex-plugin --save-dev
# 使用 yarn:
yarn add create-flex-plugin --dev
# 安装成功后命令在 ./node_modules/.bin/create-flex-plugin 路径
# 或者使用 npm 的 npx 命令：
npx create-flex-plugin
# 也可以使用 yarn：
yarn create-flex-plugin
```

事实上，即使没有提前安装 CLI，`npx` 命令也可以执行。只需运行 `npx create-flex-plugin` 命令，`npx` 会优先使用本地或全局安装的版本，如果找不到，它会将其下载到缓存中。

从 `npm` 6.1 版本开始，使用 `npm init` 和 `yarn` 支持使用名为`create-*` 的 CLI 来启动项目的方法。例如，对于我们的`create-flex-plugin`，我们真正需要调用的是：

```
# 使用 Node.js
npm init flex-plugin
# 使用 Yarn:
yarn create flex-plugin
```

## 创建你的第一个 CLI

如果你希望按照视频教程进行操作，[请查看我们在 YouTube 上的教程](https://www.youtube.com/watch?v=s2h28p4s-Xs).

现在我们介绍了你可能想要使用 Node.js 创建 CLI 的原因，现在让我们来构建一个 CLI。我们将在本教程中使用 `npm`，但是绝大部分都有相同作用的 `yarn` 命令。确保你在系统上安装了 [Node.js](https://nodejs.org/en/download/) 和 [`npm`](https://www.npmjs.com/)。

在本教程中，我们将创建一个 CLI，通过运行 `npm init @your-username/project`，创建一个你自己偏好的新项目。

通过如下命令创建一个新的 Node.js 项目：

```
mkdir create-project && cd create-project
npm init --yes
```

然后在项目的根目录中创建一个名为 `src/` 的目录，并在其中创建 `cli.js` 文件，代码如下：

```
export function cli(args) {
 console.log(args);
}
```

这将是我们稍后解析逻辑然后触发实际业务逻辑的部分。接下来，我们需要为 CLI 创建入口。在项目的根目录中创建一个新目录 `bin/`，并在其中创建一个名为 `create-project` 的新文件。将以下代码行放入其中：

```
#!/usr/bin/env node

require = require('esm')(module /*, options*/);
require('../src/cli').cli(process.argv);
```

这个小代码片段一共做了两件事情。首先，我们引入了一个名为 `esm` 的模块，它可以让我们在其他文件中使用 `import`。这与构建 CLI 没有直接关系，但我们将在本教程中使用 [ES 模块](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)，`esm` 包允许我们这样做，而无需在没有支持的情况下转换 Node.js 版本。然后我们引入我们的 `cli.js` 文件并调用其暴露的 `cli` 函数，并传入 [`process.argv`](https://nodejs.org/api/process.html#process_process_argv) 参数，该参数是从命令行传递给该脚本的全部参数的数组。

在我们测试脚本之前，需要安装 `esm` 依赖，执行如下命令：

```
npm install esm
```

我们还必须告知包管理器我们需要暴露的 CLI 脚本。通过在 `package.json` 中添加合适的条目来完成此操作。不要忘记更新 `description`、`name`、`keyword` 和 `main` 属性：

```
{
 "name": "@your_npm_username/create-project",
 "version": "1.0.0",
 "description": "A CLI to bootstrap my new projects",
 "main": "src/index.js",
 "bin": {
   "@your_npm_username/create-project": "bin/create-project",
   "create-project": "bin/create-project"
 },
 "publishConfig": {
   "access": "public"
 },
 "scripts": {
   "test": "echo \"Error: no test specified\" && exit 1"
 },
 "keywords": [
   "cli",
   "create-project"
 ],
 "author": "YOUR_AUTHOR",
 "license": "MIT",
 "dependencies": {
   "esm": "^3.2.18"
 }
}
```

我们来看 `bin` 这个属性，值为一个包含两个键值对的对象。它们定义了你的包管理器将要安装的 CLI 命令。在我们的例子中，我们为相同脚本注册了两条命令。一条命令使用我们自己用户名的 `npm` 作用域，另一条通用的 `create-project` 命令方便使用。

现在我们完成了这项工作，我们可以测试我们的脚本。要做到这一点，最简单的方法是使用 [`npm link`](https://docs.npmjs.com/cli/link.html) 命令。在项目内的终端中运行：

```
npm link
```

这将当前项目链接到全局执行环境，因此在更新代码时无需重新运行它。运行 `npm link` 后，你就可以使用 CLI 命令了。试试运行：

```
create-project
```

应该可以看到类似于如下的输出：

```
[ '/usr/local/Cellar/node/11.6.0/bin/node',
  '/Users/dkundel/dev/create-project/bin/create-project' ]
```

Note that both paths will be different for you depending on where your project lies and where you have Node.js installed. This array will be longer with every argument that you add to this. Try running:

```
create-project --yes
```

And the output should reflect the new argument:

```
[ '/usr/local/Cellar/node/11.6.0/bin/node',
  '/Users/dkundel/dev/create-project/bin/create-project',
  '--yes' ]
```

## Parsing Arguments and Handling Input

We are now ready to parse the arguments that are being passed to our script and we can start making sense of them. Our CLI will support one argument and a few options:

*   `[template]`: We'll support different templates out of the box. If this is not passed we'll prompt the user to select a template
*   `--git`: This will run `git init` to instantiate a new git project
*   `--install`: This will automatically install all the dependencies for the project
*   `--yes`: This will skip all prompts and go for default options

For our project we'll use `inquirer` to prompt for missing values and the `arg` library to parse our CLI arguments. Install the missing dependencies by running:

```
npm install inquirer arg
```

Let's first write the logic that will parse our arguments into an `options` object that we can work with. Add the following code to your `cli.js`:

```
import arg from 'arg';

function parseArgumentsIntoOptions(rawArgs) {
 const args = arg(
   {
     '--git': Boolean,
     '--yes': Boolean,
     '--install': Boolean,
     '-g': '--git',
     '-y': '--yes',
     '-i': '--install',
   },
   {
     argv: rawArgs.slice(2),
   }
 );
 return {
   skipPrompts: args['--yes'] || false,
   git: args['--git'] || false,
   template: args._[0],
   runInstall: args['--install'] || false,
 };
}

export function cli(args) {
 let options = parseArgumentsIntoOptions(args);
 console.log(options);
}
```

Try running `create-project --yes` and you should see `skipPrompt` to turn to `true` or try passing another argument in like `create-project cli` and the `template` property should be set.

Now that we are able to parse the CLI arguments, we'll need to add the functionality to prompt for the missing information as well as skip the prompt and resort to default arguments if the `--yes` flag is passed. Add the following code to your `cli.js` file:

```
import arg from 'arg';
import inquirer from 'inquirer';

function parseArgumentsIntoOptions(rawArgs) {
// ...
}

async function promptForMissingOptions(options) {
 const defaultTemplate = 'JavaScript';
 if (options.skipPrompts) {
   return {
     ...options,
     template: options.template || defaultTemplate,
   };
 }

 const questions = [];
 if (!options.template) {
   questions.push({
     type: 'list',
     name: 'template',
     message: 'Please choose which project template to use',
     choices: ['JavaScript', 'TypeScript'],
     default: defaultTemplate,
   });
 }

 if (!options.git) {
   questions.push({
     type: 'confirm',
     name: 'git',
     message: 'Initialize a git repository?',
     default: false,
   });
 }

 const answers = await inquirer.prompt(questions);
 return {
   ...options,
   template: options.template || answers.template,
   git: options.git || answers.git,
 };
}

export async function cli(args) {
 let options = parseArgumentsIntoOptions(args);
 options = await promptForMissingOptions(options);
 console.log(options);
}
```

Save the file and run `create-project` and you should be prompted with a template selection prompt:

![](https://s3.amazonaws.com/com.twilio.prod.twilio-docs/images/yfGSsiUKImPbvqn6_YlDO5TyLhCF9qT953-6KN4vStg5Wl.width-500.png)

And afterwards you'll be prompted with a question whether you want to initialize `git`. Once you selected both you should see output like this printed:

```
{ skipPrompts: false,
  git: false,
  template: 'JavaScript',
  runInstall: false }
```

Try to run the same command with `-y` and the prompts should be skipped. Instead you'll immediately see the determined options output.

![](https://s3.amazonaws.com/com.twilio.prod.twilio-docs/images/JhCdQpSDKZzTbIW7stxZScvwr5ak8IZzjvlyLtPihPovb-.width-500.png)

## Writing the Logic

Now that we are able to determine the respective options through prompts and command-line arguments, let's write the actual logic that will create our projects. Our CLI will write into an existing directory similar to `npm init` and it will copy all files from a `templates` directory in our project. We'll allow the target directory to be also modified via the options in case you want to re-use the same logic inside another project.

Before we write the actual logic, create a `templates` directory in the root of our project and place two directories with the names `typescript` and `javascript` into it. Those are the lower-cased versions of the two values that we prompted the user to pick from. This post will use these names but feel free to use other names you'd like. Inside that directory place any `package.json` that you would like to use as the base of your project and any kind of files you want to have copied into your project. Our code will later simply copy those files into the new project. If you need some inspiration, you can check out my files at github.com/dkundel/create-project.

In order to do recursive copying of the files we'll use a library called `ncp`. This library supports recursive copying cross-platform and even has a flag to force override existing files. Additionally we'll install `chalk` for colored output. To install the dependencies run:

```
npm install ncp chalk
```

We'll place all of our core logic into a `main.js` file inside the `src/` directory of our project. Create the new file and add the following code:

```
import chalk from 'chalk';
import fs from 'fs';
import ncp from 'ncp';
import path from 'path';
import { promisify } from 'util';

const access = promisify(fs.access);
const copy = promisify(ncp);

async function copyTemplateFiles(options) {
 return copy(options.templateDirectory, options.targetDirectory, {
   clobber: false,
 });
}

export async function createProject(options) {
 options = {
   ...options,
   targetDirectory: options.targetDirectory || process.cwd(),
 };

 const currentFileUrl = import.meta.url;
 const templateDir = path.resolve(
   new URL(currentFileUrl).pathname,
   '../../templates',
   options.template.toLowerCase()
 );
 options.templateDirectory = templateDir;

 try {
   await access(templateDir, fs.constants.R_OK);
 } catch (err) {
   console.error('%s Invalid template name', chalk.red.bold('ERROR'));
   process.exit(1);
 }

 console.log('Copy project files');
 await copyTemplateFiles(options);

 console.log('%s Project ready', chalk.green.bold('DONE'));
 return true;
}
```

This code will export a new function called `createProject` that will first check if the specified template is indeed an available template, by checking the `read` access (`fs.constants.R_OK`) using [`fs.access`](https://nodejs.org/api/fs.html#fs_fs_access_path_mode_callback) and then copy the files into the target directory using `ncp`. Additionally we'll log some colored output saying `DONE Project ready` when we successfully copied the files.

Afterwards update your `cli.js` to call the new `createProject` function:

```
import arg from 'arg';
import inquirer from 'inquirer';
import { createProject } from './main';

function parseArgumentsIntoOptions(rawArgs) {
// ...
}

async function promptForMissingOptions(options) {
// ...
}

export async function cli(args) {
 let options = parseArgumentsIntoOptions(args);
 options = await promptForMissingOptions(options);
 await createProject(options);
}
```

To test our progress, create a new directory somewhere like `~/test-dir` on your system and run inside it the command using one of your templates. For example:

```
create-project typescript --git
```

You should see a confirmation that the project has been created and the files should be copied over to the directory.

![](https://s3.amazonaws.com/com.twilio.prod.twilio-docs/images/hJhXfoE6BvWWHNFzomCs4YD5D4fLUWQqHmR-am2wzAGszS.width-500.png)

Now there are two more steps we want our CLI to do. We want to optionally initialize `git` and install our dependencies. For this we'll use three more dependencies:

*   [`execa`](http://npm.im/execa) which allows us to easily run external commands like `git`
*   [`pkg-install`](http://npm.im/pkg-install) to trigger either `yarn install` or `npm install` depending on what the user uses
*   [`listr`](http://npm.im/listr) which let's us specify a list of tasks and gives the user a neat progress overview

Install the dependencies by running:

```
npm install execa pkg-install listr
```

Afterwards update your `main.js` to contain the following code:

```
import chalk from 'chalk';
import fs from 'fs';
import ncp from 'ncp';
import path from 'path';
import { promisify } from 'util';
import execa from 'execa';
import Listr from 'listr';
import { projectInstall } from 'pkg-install';

const access = promisify(fs.access);
const copy = promisify(ncp);

async function copyTemplateFiles(options) {
 return copy(options.templateDirectory, options.targetDirectory, {
   clobber: false,
 });
}

async function initGit(options) {
 const result = await execa('git', ['init'], {
   cwd: options.targetDirectory,
 });
 if (result.failed) {
   return Promise.reject(new Error('Failed to initialize git'));
 }
 return;
}

export async function createProject(options) {
 options = {
   ...options,
   targetDirectory: options.targetDirectory || process.cwd()
 };

 const templateDir = path.resolve(
   new URL(import.meta.url).pathname,
   '../../templates',
   options.template
 );
 options.templateDirectory = templateDir;

 try {
   await access(templateDir, fs.constants.R_OK);
 } catch (err) {
   console.error('%s Invalid template name', chalk.red.bold('ERROR'));
   process.exit(1);
 }

 const tasks = new Listr([
   {
     title: 'Copy project files',
     task: () => copyTemplateFiles(options),
   },
   {
     title: 'Initialize git',
     task: () => initGit(options),
     enabled: () => options.git,
   },
   {
     title: 'Install dependencies',
     task: () =>
       projectInstall({
         cwd: options.targetDirectory,
       }),
     skip: () =>
       !options.runInstall
         ? 'Pass --install to automatically install dependencies'
         : undefined,
   },
 ]);

 await tasks.run();
 console.log('%s Project ready', chalk.green.bold('DONE'));
 return true;
}
```

This will run `git init` whenever `--git` is passed or the user chooses `git` in the prompt and it will run `npm install` or `yarn` whenever the user passes `--install`, otherwise it will skip the task with a message informing the user to pass `--install` if they want automatic install.

Give it a try by deleting your existing test folder first and creating a new one. Then run:

```
create-project typescript --git --install
```

You should see now both a `.git` folder in your folder indicating that `git` has been initialized and a `node_modules` folder with your dependencies that were specified in the `package.json` installed.

![](https://s3.amazonaws.com/com.twilio.prod.twilio-docs/images/_vaH2-wgo0HxH7o6NVcQwlb-h7MihVzFVO_6MsTcw71qB8.width-500.png)

Congratulations you got your first CLI ready to go!

![](https://s3.amazonaws.com/com.twilio.prod.twilio-docs/original_images/Hlw2ROeuFERjiDODTmaGu6z7YtmcGN0yTWiPeRLRRr6EENm7HJ8s3laAGZGdd54NefTAJPut5nZCDe)

If you want to make your code consumable as an actual module so that others can reuse your logic in their code, we'll have to add an `index.js` file to our `src/` directory that exposes the content from `main.js`:

```
require = require('esm')(module);
require('../src/cli').cli(process.argv);
```

## What's Next?

Now that you have your CLI code ready there are a few ways you can go from here. If you just want to use this yourself and don't want to share it with the world you can just keep on going along the path of using `npm link`. In fact try running `npm init project` and it should trigger your code.

If you want to share your templates with the world either push your code to GitHub and consume it from there or even better push it as a scoped package to the `npm` registry with [`npm publish`](https://docs.npmjs.com/cli/publish). Before you do so, you should make sure to add a `files` key in your `package.json` to specify which files should be published.

```
},
 "files": [
   "bin/",
   "src/",
   "templates/"
 ]
}
```

If you want to check which files will be published, run `npm pack --dry-run` and check the output. Afterwards use `npm publish` to publish your CLI. You can find my project under [`@dkundel/create-project`](http://npm.im/@dkundel/create-project) or try run `npm init @dkundel/project`.

There's also lots of functionality that you can add. In my case I added some additional dependencies that will create a `LICENSE`, `CODE_OF_CONDUCT.md` and `.gitignore` file for me. You can [find the source code for it on GitHub](http://github.com/dkundel/create-project) or check out some of the libraries mentioned above for some additional functionality. If you have a library I didn't list and you believe it should totally be in the list or if you want to show me your own CLI, feel free to send me a message!

*   Email: [dkundel@twilio.com](mailto:dkundel@twilio.com)
*   Twitter: [@dkundel](https://twitter.com/dkundel?lang=en)
*   GitHub: [dkundel](https://github.com/dkundel)
*   [dkundel.com](https://dkundel.com/)


