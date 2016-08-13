title: Linux下安装node的问题
targs:
    - Linux
    - node
---

**Linux** 下安装node的方式十分简单,当然我不会傻到源码编译安装.直接从node的官网上下载
已经编译好的的包.将`bin/`目录下的`node`和`npm`复制到PATH的路径下.

```
sudo cp node /usr/local/bin/
sudo cp npm /usr/local/npm/
```

这样安装之后,`node`是可以正常使用了.使用`node -v`也可以正常的输出的版本号.但是我使用
`npm`的时候,却失败了.

```
module.js:327
throw err;
^
Error: Cannot find module 'npmlog'
at Function.Module._resolveFilename (module.js:338:15)
at Function.Module._load (module.js:280:25)
at Module.require (module.js:362:17)
at require (module.js:378:17)
at /usr/local/bin/npm:19:11
at Object. (/usr/local/bin/npm:87:3)
at Module._compile (module.js:449:26)
at Object.Module._extensions..js (module.js:467:10)
at Module.load (module.js:356:32)
at Function.Module._load (module.js:312:12)
```

找不到一个叫`npmlog`的模块,我突然想起来.`npm`是有一个全局的配置的,在 **Linux** 下,应该
是在其全局`node_modules`下,这个模块应该是`npm`所必须的.

仔细查阅了`npm`在github上的issue,原来提供了一个脚本将`npm`必须的东西安装:
```
curl -0 -L http://npmjs.org/install.sh | sudo sh
```

## 总结

遇上问题,要查找一下这个开源项目的github上的issue.
