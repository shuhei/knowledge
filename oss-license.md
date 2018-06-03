# OSS License

I'm not a lawyer. Don't trust me.

- Read [Should you care about the license? (TL;DR: yes!)](https://medium.com/@vovabilonenko/licenses-of-npm-dependencies-bacaa00c8c65)
- Nice to have a `LICENSE` file in your OSS repo in addition to a license name in `package.json`, etc.

## Including copyright and license when static-linking OSS libraries

It seems that we should include copyrights and license notes even for permissive licenses when we static-link them. For example, MIT License says:

>The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

When static-linking JavaScript libraries to a bundle with webpack, I can think of two options so far.

1. Configure webpack to leave copyright & license comments in libraries.
  - https://github.com/webpack-contrib/uglifyjs-webpack-plugin/issues/222
  - This may not be enough because not all libraries have copyright or license comments in their source code.
2. Configure webpack to collect copyright and license files or the information from `package.json` in libraries, and include them in the bundle.
  - This should be able to cover more libraries than the option 1.
