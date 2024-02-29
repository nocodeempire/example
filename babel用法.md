```js
/**
 * @babel/core
 * babel的核心包
 * 主要有 
 * transformSync transformAsync  解析字符串到指定环境的代码
 * transformFileSync transformFileAsync  解析文件到指定环境的代码
 * transformFromAstSync transformFromAstAsync 解析Ast到指定环境的代码
 * ...等等
 */
import babel from '@babel/core'
/**
 * @babel/preset-env 是一个智能预设，允许你使用最新的 JavaScript，而无需微观管理目标环境需要哪些语法转换（以及可选的浏览器 polyfill）
 * 通俗讲就是对es6,7等往后的一些新功能, 如箭头函数, class, async等新功能, 解析到指定环境的一个集合;
 * 说智能预设是因为, 当我们设置了browserslist(browserslist能设置浏览器版本等)或者指定targets, 就能根据你指定的环境把代码解析至此环境支持的代码
 * 类似的集合有@babel/preset-typescript, @babel/preset-react等
 */
import presetEnv from '@babel/preset-env'

import parser from '@babel/parser';

// example1
const sourceCode = "const aa = () => console.log('NAME')"
const { code: code1 } = babel.transformSync(
  sourceCode,
  {presets: [presetEnv]}
  // {presets: [[presetEnv, {targets: {"chrome": "46"}}]]} // 指定47 箭头函数就还是箭头函数
);
console.log(code1)


// example2
const sourceCode1 = "const aa = () => console.log('NAME')"
const ast1 = parser.parse(sourceCode1, {})
const { code } = babel.transformFromAst(ast1, null, {
  presets: [presetEnv],
});
console.log(code);






/**
 * babel原理 parse -> transform -> generate
 * parse 把代码解析成Ast(抽象语法树), 就是个对象, https://astexplorer.net/
 * transform 基于解析出的Ast做增删改查, 可以实现如在所有方法中打印111(埋点功能), 抓取所有中文进行翻译, 实现各类客制化的插件需求...
 * generate 对Ast再次处理成指定编译环境下的代码
 */
import parser from '@babel/parser';
import traverse from '@babel/traverse';
import generator from '@babel/generator';
import * as t from '@babel/types';

// example 以下例子主要展示在console.log打印中把当前行列也打印
const sourceCode2 = `
  console.log(1);

  const fun = () => console.log(2);

  const comp = () => {
    return <div>{console.log(3)}</div>
  }
`;

const targetCalleeName = 'console.log';

const ast = parser.parse(sourceCode2, {
  sourceType: 'unambiguous',
  plugins: ['jsx']
})

traverse.default(ast,  {
  CallExpression (path, state) {
    const calleeName = generator.default(path.node.callee).code;
    if (targetCalleeName.includes(calleeName)) {
      const { line, column } = path.node.loc.start;
      path.node.arguments.unshift(t.stringLiteral(`LineRow: (${line}, ${column})`))
    }
  }
});

const { code } = generator.default(ast);

console.log(code);






import babel from '@babel/core'
import template from "@babel/template";
import generator from '@babel/generator'
import * as t from '@babel/types'
import parser from '@babel/parser'


// example1 先了解下@babel/template
const fn = template.default(`function aa() {console.log(%%NAME%%)}`); // 转成ast

const ast = fn({  // 如果有需要动态替换的话可以这样操作
  NAME: t.stringLiteral('Evan')
})

const { code } = generator.default(ast);

console.log(code);



// example2  实现一个在每个方法中打印方法名的插件;
/**
 * 插件原理
 * 我们在webpack等打包工具中处理js,ts,jsx等js相关的, 肯定是用了babel的, 此时在plugins中加入自己写的插件应该就可以
 * 在用插件前babel已经把所有代码先处理成ast了, 所以插件只需要去处理这颗ast树即可
 */

// 假设这是我们插件需要的数据
const str = ` 
  const a = 'a';
  function aa() {console.log('原始的')};
  const bb = () => {};
`

// 插件
export default function testPlugin() {
  return {
    visitor: {
      // Function 就可以表示 FunctionDeclaration, FunctionExpression, ArrowFunctionExpression, ObjectMethod 和 ClassMethod 这 5 中类型，因为 Function 是这 5 中类型的别名
      Function(path, state) {
        const bodyPath = path.get('body');
        if (bodyPath.isBlockStatement()) {
          bodyPath.node.body.unshift(template.default.ast('console.log("在方法中加入")'));
        }
      }
    }
  };
}

// Test
const ast2 = parser.parse(str, {})
const { code: c1 } = babel.transformFromAstSync(ast2, str, {plugins: [testPlugin]})
console.log(c1)


```
