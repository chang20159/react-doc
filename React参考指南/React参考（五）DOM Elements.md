```
[2017-05-26 17:32:45.367] [INFO] prism-monitor-logstore-web - Request ID:  03fc92b59f4454fb2f95f150ded372eb
[2017-05-26 17:32:45.368] [INFO] prism-monitor-logstore-web - 查询条件 {}
[2017-05-26 17:32:45.374] [TRACE] prism-monitor-logstore-web - Executing (default): SELECT `Id` AS `id`, `PageUrl` AS `target`, `FromUrl` AS `from`, `Type` AS `type`, `BrowserName` AS `browserName`, `BrowserVersion` AS `browserVersion`, `Ip` AS `ip`, `Title` AS `title`, `UserAgent` AS `userAgent`, `Row` AS `rowNum`, `Col` AS `colNum`, `DomType` AS `domType`, `DomId` AS `domId`, `DomClass` AS `domClass`, `DomXPosition` AS `domX`, `DomYPosition` AS `domY`, `Platform` AS `platform`, `Message` AS `msg`, `Attribute` AS `attribute`, `ApplyId` AS `applyId`, `Level` AS `level`, `Env` AS `env`, `Os` AS `os`, `Engine` AS `engine`, `AddTime` AS `addTime`, `UpdateTime` AS `updateTime` FROM `FT_Log` AS `log` ORDER BY addTime desc;
[2017-05-26 17:35:28.602] [INFO] prism-monitor-logstore-web - { SequelizeDatabaseError: read ETIMEDOUT
    at Query.formatError (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/dialects/mysql/query.js:175:14)
    at Query._callback (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/dialects/mysql/query.js:49:21)
    at Query.Sequence.end (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/mysql/lib/protocol/sequences/Sequence.js:86:24)
    at Protocol.handleNetworkError (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/mysql/lib/protocol/Protocol.js:364:14)
    at Connection._handleNetworkError (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/mysql/lib/Connection.js:428:18)
    at emitOne (events.js:96:13)
    at Socket.emit (events.js:188:7)
    at emitErrorNT (net.js:1276:8)
    at _combinedTickCallback (internal/process/next_tick.js:74:11)
    at process._tickDomainCallback (internal/process/next_tick.js:122:9)
From previous event:
    at Query.run (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/dialects/mysql/query.js:39:17)
    at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/sequelize.js:849:20
    at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/retry-as-promised/index.js:40:21
From previous event:
    at retryAsPromised (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/retry-as-promised/index.js:30:10)
    at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/sequelize.js:848:12
From previous event:
    at Promise.then (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/promise.js:21:17)
    at Model.bulkCreate (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/model.js:2154:6)
    at LogDao._callee2$ (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/lib/dao/LogDao.js:76:50)
    at tryCatch (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/regenerator-runtime/runtime.js:65:40)
    at GeneratorFunctionPrototype.invoke [as _invoke] (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/regenerator-runtime/runtime.js:303:22)
    at GeneratorFunctionPrototype.prototype.(anonymous function) [as next] (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/regenerator-runtime/runtime.js:117:21)
    at step (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/lib/dao/LogDao.js:23:201)
    at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/lib/dao/LogDao.js:23:457
    at Promise.F (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/core-js/library/modules/_export.js:35:28)
    at LogDao.<anonymous> (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/lib/dao/LogDao.js:23:99)
    at LogDao.save (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/lib/dao/LogDao.js:102:26)
    at LogService._callee$ (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/lib/services/LogService.js:103:48)
    at tryCatch (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/regenerator-runtime/runtime.js:65:40)
    at GeneratorFunctionPrototype.invoke [as _invoke] (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/regenerator-runtime/runtime.js:303:22)
    at GeneratorFunctionPrototype.prototype.(anonymous function) [as next] (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/regenerator-runtime/runtime.js:117:21)
    at step (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/lib/services/LogService.js:31:201)
    at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/lib/services/LogService.js:31:457
    at Promise.F (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/core-js/library/modules/_export.js:35:28)
    at LogService.<anonymous> (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/lib/services/LogService.js:31:99)
  name: 'SequelizeDatabaseError',
  message: 'read ETIMEDOUT',
  parent:
   { Error: read ETIMEDOUT
       at exports._errnoException (util.js:1026:11)
       at TCP.onread (net.js:569:26)
       --------------------
       at Protocol._enqueue (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/mysql/lib/protocol/Protocol.js:141:48)
       at Connection.query (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/mysql/lib/Connection.js:208:25)
       at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/dialects/mysql/query.js:40:21
       at Promise._execute (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/debuggability.js:300:9)
       at Promise._resolveFromExecutor (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:483:18)
       at new Promise (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:79:10)
       at Query.run (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/dialects/mysql/query.js:39:17)
       at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/sequelize.js:849:20
       at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/retry-as-promised/index.js:40:21
       at Promise._execute (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/debuggability.js:300:9)
       at Promise._resolveFromExecutor (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:483:18)
       at new Promise (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:79:10)
       at retryAsPromised (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/retry-as-promised/index.js:30:10)
       at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/sequelize.js:848:12
       at tryCatcher (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/util.js:16:23)
       at Promise._settlePromiseFromHandler (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:512:31)
     code: 'ETIMEDOUT',
     errno: 'ETIMEDOUT',
     syscall: 'read',
     fatal: true,
     sql: 'INSERT INTO `FT_Log` (`Id`,`PageUrl`,`Type`,`BrowserName`,`BrowserVersion`,`Ip`,`Title`,`UserAgent`,`Row`,`Col`,`Platform`,`Message`,`ApplyId`,`Env`,`Os`,`Engine`,`AddTime`,`UpdateTime`) VALUES (NULL,\'http://www.uedfamily.com/news/list/2001.html\',\'jsError\',\'Firefox\',\'52.0\',\'127.0.0.1\',\'千钧一发！动车进站瞬间女子跳轨 客运员死命拽回\',\'Mozilla/5.0 (Macintosh; Intel Mac OS X 10.12; rv:52.0) Gecko/20100101 Firefox/52.0\',\'6774\',\'4252\',\'Apple Mac\',\'Failed Context Types: Calling PropTypes validators directly is not supported by the `prop-types` package. Use `PropTypes.checkPropTypes()` to call them. Read more at http://fb.me/use-check-prop-types\',\'10000010\',\'Mac\',\'OS X\',\'Gecko\',\'2017-07-17 04:08:13\',\'2017-05-26 17:19:59\');' },
  original:
   { Error: read ETIMEDOUT
       at exports._errnoException (util.js:1026:11)
       at TCP.onread (net.js:569:26)
       --------------------
       at Protocol._enqueue (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/mysql/lib/protocol/Protocol.js:141:48)
       at Connection.query (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/mysql/lib/Connection.js:208:25)
       at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/dialects/mysql/query.js:40:21
       at Promise._execute (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/debuggability.js:300:9)
       at Promise._resolveFromExecutor (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:483:18)
       at new Promise (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:79:10)
       at Query.run (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/dialects/mysql/query.js:39:17)
       at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/sequelize.js:849:20
       at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/retry-as-promised/index.js:40:21
       at Promise._execute (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/debuggability.js:300:9)
       at Promise._resolveFromExecutor (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:483:18)
       at new Promise (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:79:10)
       at retryAsPromised (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/retry-as-promised/index.js:30:10)
       at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/sequelize.js:848:12
       at tryCatcher (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/util.js:16:23)
       at Promise._settlePromiseFromHandler (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:512:31)
     code: 'ETIMEDOUT',
     errno: 'ETIMEDOUT',
     syscall: 'read',
     fatal: true,
     sql: 'INSERT INTO `FT_Log` (`Id`,`PageUrl`,`Type`,`BrowserName`,`BrowserVersion`,`Ip`,`Title`,`UserAgent`,`Row`,`Col`,`Platform`,`Message`,`ApplyId`,`Env`,`Os`,`Engine`,`AddTime`,`UpdateTime`) VALUES (NULL,\'http://www.uedfamily.com/news/list/2001.html\',\'jsError\',\'Firefox\',\'52.0\',\'127.0.0.1\',\'千钧一发！动车进站瞬间女子跳轨 客运员死命拽回\',\'Mozilla/5.0 (Macintosh; Intel Mac OS X 10.12; rv:52.0) Gecko/20100101 Firefox/52.0\',\'6774\',\'4252\',\'Apple Mac\',\'Failed Context Types: Calling PropTypes validators directly is not supported by the `prop-types` package. Use `PropTypes.checkPropTypes()` to call them. Read more at http://fb.me/use-check-prop-types\',\'10000010\',\'Mac\',\'OS X\',\'Gecko\',\'2017-07-17 04:08:13\',\'2017-05-26 17:19:59\');' },
  sql: 'INSERT INTO `FT_Log` (`Id`,`PageUrl`,`Type`,`BrowserName`,`BrowserVersion`,`Ip`,`Title`,`UserAgent`,`Row`,`Col`,`Platform`,`Message`,`ApplyId`,`Env`,`Os`,`Engine`,`AddTime`,`UpdateTime`) VALUES (NULL,\'http://www.uedfamily.com/news/list/2001.html\',\'jsError\',\'Firefox\',\'52.0\',\'127.0.0.1\',\'千钧一发！动车进站瞬间女子跳轨 客运员死命拽回\',\'Mozilla/5.0 (Macintosh; Intel Mac OS X 10.12; rv:52.0) Gecko/20100101 Firefox/52.0\',\'6774\',\'4252\',\'Apple Mac\',\'Failed Context Types: Calling PropTypes validators directly is not supported by the `prop-types` package. Use `PropTypes.checkPropTypes()` to call them. Read more at http://fb.me/use-check-prop-types\',\'10000010\',\'Mac\',\'OS X\',\'Gecko\',\'2017-07-17 04:08:13\',\'2017-05-26 17:19:59\');' }
[2017-05-26 17:36:02.972] [INFO] prism-monitor-logstore-web - { SequelizeDatabaseError: read ETIMEDOUT
    at Query.formatError (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/dialects/mysql/query.js:175:14)
    at Query._callback (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/dialects/mysql/query.js:49:21)
    at Query.Sequence.end (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/mysql/lib/protocol/sequences/Sequence.js:86:24)
    at Protocol.handleNetworkError (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/mysql/lib/protocol/Protocol.js:364:14)
    at Connection._handleNetworkError (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/mysql/lib/Connection.js:428:18)
    at emitOne (events.js:96:13)
    at Socket.emit (events.js:188:7)
    at emitErrorNT (net.js:1276:8)
    at _combinedTickCallback (internal/process/next_tick.js:74:11)
    at process._tickDomainCallback (internal/process/next_tick.js:122:9)
From previous event:
    at Query.run (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/dialects/mysql/query.js:39:17)
    at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/sequelize.js:849:20
    at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/retry-as-promised/index.js:40:21
From previous event:
    at retryAsPromised (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/retry-as-promised/index.js:30:10)
    at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/sequelize.js:848:12
From previous event:
    at Promise.then (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/promise.js:21:17)
    at Model.bulkCreate (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/model.js:2154:6)
    at LogDao._callee2$ (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/lib/dao/LogDao.js:76:50)
    at tryCatch (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/regenerator-runtime/runtime.js:65:40)
    at GeneratorFunctionPrototype.invoke [as _invoke] (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/regenerator-runtime/runtime.js:303:22)
    at GeneratorFunctionPrototype.prototype.(anonymous function) [as next] (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/regenerator-runtime/runtime.js:117:21)
    at step (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/lib/dao/LogDao.js:23:201)
    at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/lib/dao/LogDao.js:23:457
    at Promise.F (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/core-js/library/modules/_export.js:35:28)
    at LogDao.<anonymous> (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/lib/dao/LogDao.js:23:99)
    at LogDao.save (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/lib/dao/LogDao.js:102:26)
    at LogService._callee$ (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/lib/services/LogService.js:103:48)
    at tryCatch (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/regenerator-runtime/runtime.js:65:40)
    at GeneratorFunctionPrototype.invoke [as _invoke] (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/regenerator-runtime/runtime.js:303:22)
    at GeneratorFunctionPrototype.prototype.(anonymous function) [as next] (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/regenerator-runtime/runtime.js:117:21)
    at step (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/lib/services/LogService.js:31:201)
    at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/lib/services/LogService.js:31:457
    at Promise.F (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/core-js/library/modules/_export.js:35:28)
    at LogService.<anonymous> (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/lib/services/LogService.js:31:99)
  name: 'SequelizeDatabaseError',
  message: 'read ETIMEDOUT',
  parent:
   { Error: read ETIMEDOUT
       at exports._errnoException (util.js:1026:11)
       at TCP.onread (net.js:569:26)
       --------------------
       at Protocol._enqueue (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/mysql/lib/protocol/Protocol.js:141:48)
       at Connection.query (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/mysql/lib/Connection.js:208:25)
       at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/dialects/mysql/query.js:40:21
       at Promise._execute (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/debuggability.js:300:9)
       at Promise._resolveFromExecutor (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:483:18)
       at new Promise (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:79:10)
       at Query.run (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/dialects/mysql/query.js:39:17)
       at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/sequelize.js:849:20
       at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/retry-as-promised/index.js:40:21
       at Promise._execute (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/debuggability.js:300:9)
       at Promise._resolveFromExecutor (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:483:18)
       at new Promise (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:79:10)
       at retryAsPromised (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/retry-as-promised/index.js:30:10)
       at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/sequelize.js:848:12
       at tryCatcher (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/util.js:16:23)
       at Promise._settlePromiseFromHandler (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:512:31)
     code: 'ETIMEDOUT',
     errno: 'ETIMEDOUT',
     syscall: 'read',
     fatal: true,
     sql: 'INSERT INTO `FT_Log` (`Id`,`PageUrl`,`Type`,`BrowserName`,`BrowserVersion`,`Ip`,`Title`,`UserAgent`,`Row`,`Col`,`Platform`,`Message`,`ApplyId`,`Env`,`Os`,`Engine`,`AddTime`,`UpdateTime`) VALUES (NULL,\'https://www.baidu.com/msgList/6977.html\',\'jsError\',\'Chrome\',\'21.0.1180.71\',\'127.0.0.1\',\'《悟空传》“悟空是我”特辑\',\'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.1 (KHTML, like Gecko) Chrome/21.0.1180.71 Safari/537.1 LBBROWSER\',\'2295\',\'4067\',\'Microsoft Windows\',\'Failed Context Types: Calling PropTypes validators directly is not supported by the `prop-types` package. Use `PropTypes.checkPropTypes()` to call them. Read more at http://fb.me/use-check-prop-types\',\'10000010\',\'Windows\',\'Windows 7\',\'Webkit\',\'Invalid date\',\'2017-05-26 17:20:34\');' },
  original:
   { Error: read ETIMEDOUT
       at exports._errnoException (util.js:1026:11)
       at TCP.onread (net.js:569:26)
       --------------------
       at Protocol._enqueue (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/mysql/lib/protocol/Protocol.js:141:48)
       at Connection.query (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/mysql/lib/Connection.js:208:25)
       at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/dialects/mysql/query.js:40:21
       at Promise._execute (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/debuggability.js:300:9)
       at Promise._resolveFromExecutor (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:483:18)
       at new Promise (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:79:10)
       at Query.run (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/dialects/mysql/query.js:39:17)
       at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/sequelize.js:849:20
       at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/retry-as-promised/index.js:40:21
       at Promise._execute (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/debuggability.js:300:9)
       at Promise._resolveFromExecutor (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:483:18)
       at new Promise (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:79:10)
       at retryAsPromised (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/retry-as-promised/index.js:30:10)
       at /data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/sequelize/lib/sequelize.js:848:12
       at tryCatcher (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/util.js:16:23)
       at Promise._settlePromiseFromHandler (/data/webapps/prism-monitor-logstore-web/releases/20170526170201/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:512:31)
     code: 'ETIMEDOUT',
     errno: 'ETIMEDOUT',
     syscall: 'read',
     fatal: true,
     sql: 'INSERT INTO `FT_Log` (`Id`,`PageUrl`,`Type`,`BrowserName`,`BrowserVersion`,`Ip`,`Title`,`UserAgent`,`Row`,`Col`,`Platform`,`Message`,`ApplyId`,`Env`,`Os`,`Engine`,`AddTime`,`UpdateTime`) VALUES (NULL,\'https://www.baidu.com/msgList/6977.html\',\'jsError\',\'Chrome\',\'21.0.1180.71\',\'127.0.0.1\',\'《悟空传》“悟空是我”特辑\',\'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.1 (KHTML, like Gecko) Chrome/21.0.1180.71 Safari/537.1 LBBROWSER\',\'2295\',\'4067\',\'Microsoft Windows\',\'Failed Context Types: Calling PropTypes validators directly is not supported by the `prop-types` package. Use `PropTypes.checkPropTypes()` to call them. Read more at http://fb.me/use-check-prop-types\',\'10000010\',\'Windows\',\'Windows 7\',\'Webkit\',\'Invalid date\',\'2017-05-26 17:20:34\');' },
  sql: 'INSERT INTO `FT_Log` (`Id`,`PageUrl`,`Type`,`BrowserName`,`BrowserVersion`,`Ip`,`Title`,`UserAgent`,`Row`,`Col`,`Platform`,`Message`,`ApplyId`,`Env`,`Os`,`Engine`,`AddTime`,`UpdateTime`) VALUES (NULL,\'https://www.baidu.com/msgList/6977.html\',\'jsError\',\'Chrome\',\'21.0.1180.71\',\'127.0.0.1\',\'《悟空传》“悟空是我”特辑\',\'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.1 (KHTML, like Gecko) Chrome/21.0.1180.71 Safari/537.1 LBBROWSER\',\'2295\',\'4067\',\'Microsoft Windows\',\'Failed Context Types: Calling PropTypes validators directly is not supported by the `prop-types` package. Use `PropTypes.checkPropTypes()` to call them. Read more at http://fb.me/use-check-prop-types\',\'10000010\',\'Windows\',\'Windows 7\',\'Webkit\',\'Invalid date\',\'2017-05-26 17:20:34\');' }
[2017-05-26 17:38:29.001] [INFO] prism-monitor-logstore-web - Request ID:  f560306eecbec7ac19ddec93dd2bd1fd
[2017-05-26 17:38:29.002] [INFO] prism-monitor-logstore-web - 查询条件 {}
[2017-05-26 17:38:29.010] [TRACE] prism-monitor-logstore-web - Executing (default): SELECT `Id` AS `id`, `PageUrl` AS `target`, `FromUrl` AS `from`, `Type` AS `type`, `BrowserName` AS `browserName`, `BrowserVersion` AS `browserVersion`, `Ip` AS `ip`, `Title` AS `title`, `UserAgent` AS `userAgent`, `Row` AS `rowNum`, `Col` AS `colNum`, `DomType` AS `domType`, `DomId` AS `domId`, `DomClass` AS `domClass`, `DomXPosition` AS `domX`, `DomYPosition` AS `domY`, `Platform` AS `platform`, `Message` AS `msg`, `Attribute` AS `attribute`, `ApplyId` AS `applyId`, `Level` AS `level`, `Env` AS `env`, `Os` AS `os`, `Engine` AS `engine`, `AddTime` AS `addTime`, `UpdateTime` AS `updateTime` FROM `FT_Log` AS `log` ORDER BY addTime desc;
appenv {"env":"test","deployenv":"qa","zkserver":"10.72.198.216:2181,10.72.193.216:2181,10.72.196.216:2181,10.72.194.216:2181,10.72.195.216:2181"}
Zookeeper connection: connecting...
Zookeeper connection: state(connected).
[2017-05-26 17:46:23.349] [DEBUG] prism-monitor-logstore-web - redis subscribe key: prism-logger-errorLog
[2017-05-26 17:59:14.773] [INFO] prism-monitor-logstore-web - Request ID:  8b083300819c94ba9cbcbb6bf101b718
[2017-05-26 17:59:14.776] [INFO] prism-monitor-logstore-web - 查询条件 {}
[2017-05-26 17:59:14.804] [TRACE] prism-monitor-logstore-web - Executing (default): SELECT `Id` AS `id`, `PageUrl` AS `target`, `FromUrl` AS `from`, `Type` AS `type`, `BrowserName` AS `browserName`, `BrowserVersion` AS `browserVersion`, `Ip` AS `ip`, `Title` AS `title`, `UserAgent` AS `userAgent`, `Row` AS `rowNum`, `Col` AS `colNum`, `DomType` AS `domType`, `DomId` AS `domId`, `DomClass` AS `domClass`, `DomXPosition` AS `domX`, `DomYPosition` AS `domY`, `Platform` AS `platform`, `Message` AS `msg`, `Attribute` AS `attribute`, `ApplyId` AS `applyId`, `Level` AS `level`, `Env` AS `env`, `Os` AS `os`, `Engine` AS `engine`, `AddTime` AS `addTime`, `UpdateTime` AS `updateTime` FROM `FT_Log` AS `log` ORDER BY addTime desc;
[2017-05-26 18:14:40.436] [ERROR] prism-monitor-logstore-web - query { SequelizeDatabaseError: read ETIMEDOUT
    at Query.formatError (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/sequelize/lib/dialects/mysql/query.js:175:14)
    at Query._callback (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/sequelize/lib/dialects/mysql/query.js:49:21)
    at Query.Sequence.end (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/mysql/lib/protocol/sequences/Sequence.js:86:24)
    at Protocol.handleNetworkError (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/mysql/lib/protocol/Protocol.js:364:14)
    at Connection._handleNetworkError (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/mysql/lib/Connection.js:428:18)
    at emitOne (events.js:96:13)
    at Socket.emit (events.js:188:7)
    at emitErrorNT (net.js:1276:8)
    at _combinedTickCallback (internal/process/next_tick.js:74:11)
    at process._tickDomainCallback (internal/process/next_tick.js:122:9)
From previous event:
    at Query.run (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/sequelize/lib/dialects/mysql/query.js:39:17)
    at /data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/sequelize/lib/sequelize.js:849:20
    at /data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/retry-as-promised/index.js:40:21
From previous event:
    at retryAsPromised (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/retry-as-promised/index.js:30:10)
    at ConnectionManager.<anonymous> (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/sequelize/lib/sequelize.js:848:12)
From previous event:
    at Promise.then (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/sequelize/lib/promise.js:21:17)
    at Model.findAll (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/sequelize/lib/model.js:1395:6)
    at LogDao._callee$ (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/lib/dao/LogDao.js:44:50)
    at tryCatch (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/regenerator-runtime/runtime.js:65:40)
    at GeneratorFunctionPrototype.invoke [as _invoke] (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/regenerator-runtime/runtime.js:303:22)
    at GeneratorFunctionPrototype.prototype.(anonymous function) [as next] (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/regenerator-runtime/runtime.js:117:21)
    at step (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/lib/dao/LogDao.js:23:201)
    at /data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/lib/dao/LogDao.js:23:457
    at Promise.F (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/core-js/library/modules/_export.js:35:28)
    at LogDao.<anonymous> (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/lib/dao/LogDao.js:23:99)
    at LogDao.query (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/lib/dao/LogDao.js:58:25)
    at LogService._callee2$ (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/lib/services/LogService.js:301:48)
    at tryCatch (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/regenerator-runtime/runtime.js:65:40)
    at GeneratorFunctionPrototype.invoke [as _invoke] (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/regenerator-runtime/runtime.js:303:22)
    at GeneratorFunctionPrototype.prototype.(anonymous function) [as next] (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/regenerator-runtime/runtime.js:117:21)
    at step (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/lib/services/LogService.js:31:201)
    at /data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/lib/services/LogService.js:31:457
    at Promise.F (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/core-js/library/modules/_export.js:35:28)
    at LogService.<anonymous> (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/lib/services/LogService.js:31:99)
  name: 'SequelizeDatabaseError',
  message: 'read ETIMEDOUT',
  parent:
   { Error: read ETIMEDOUT
       at exports._errnoException (util.js:1026:11)
       at TCP.onread (net.js:569:26)
       --------------------
       at Protocol._enqueue (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/mysql/lib/protocol/Protocol.js:141:48)
       at Connection.query (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/mysql/lib/Connection.js:208:25)
       at /data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/sequelize/lib/dialects/mysql/query.js:40:21
       at Promise._execute (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/debuggability.js:300:9)
       at Promise._resolveFromExecutor (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:483:18)
       at new Promise (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:79:10)
       at Query.run (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/sequelize/lib/dialects/mysql/query.js:39:17)
       at /data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/sequelize/lib/sequelize.js:849:20
       at /data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/retry-as-promised/index.js:40:21
       at Promise._execute (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/debuggability.js:300:9)
       at Promise._resolveFromExecutor (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:483:18)
       at new Promise (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:79:10)
       at retryAsPromised (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/retry-as-promised/index.js:30:10)
       at ConnectionManager.<anonymous> (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/sequelize/lib/sequelize.js:848:12)
       at ConnectionManager.tryCatcher (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/util.js:16:23)
       at Promise._settlePromiseFromHandler (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:512:31)
     code: 'ETIMEDOUT',
     errno: 'ETIMEDOUT',
     syscall: 'read',
     fatal: true,
     sql: 'SELECT `Id` AS `id`, `PageUrl` AS `target`, `FromUrl` AS `from`, `Type` AS `type`, `BrowserName` AS `browserName`, `BrowserVersion` AS `browserVersion`, `Ip` AS `ip`, `Title` AS `title`, `UserAgent` AS `userAgent`, `Row` AS `rowNum`, `Col` AS `colNum`, `DomType` AS `domType`, `DomId` AS `domId`, `DomClass` AS `domClass`, `DomXPosition` AS `domX`, `DomYPosition` AS `domY`, `Platform` AS `platform`, `Message` AS `msg`, `Attribute` AS `attribute`, `ApplyId` AS `applyId`, `Level` AS `level`, `Env` AS `env`, `Os` AS `os`, `Engine` AS `engine`, `AddTime` AS `addTime`, `UpdateTime` AS `updateTime` FROM `FT_Log` AS `log` ORDER BY addTime desc;' },
  original:
   { Error: read ETIMEDOUT
       at exports._errnoException (util.js:1026:11)
       at TCP.onread (net.js:569:26)
       --------------------
       at Protocol._enqueue (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/mysql/lib/protocol/Protocol.js:141:48)
       at Connection.query (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/mysql/lib/Connection.js:208:25)
       at /data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/sequelize/lib/dialects/mysql/query.js:40:21
       at Promise._execute (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/debuggability.js:300:9)
       at Promise._resolveFromExecutor (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:483:18)
       at new Promise (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:79:10)
       at Query.run (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/sequelize/lib/dialects/mysql/query.js:39:17)
       at /data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/sequelize/lib/sequelize.js:849:20
       at /data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/retry-as-promised/index.js:40:21
       at Promise._execute (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/debuggability.js:300:9)
       at Promise._resolveFromExecutor (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:483:18)
       at new Promise (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:79:10)
       at retryAsPromised (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/retry-as-promised/index.js:30:10)
       at ConnectionManager.<anonymous> (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/sequelize/lib/sequelize.js:848:12)
       at ConnectionManager.tryCatcher (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/util.js:16:23)
       at Promise._settlePromiseFromHandler (/data/webapps/prism-monitor-logstore-web/releases/20170526174607/prism-monitor-logstore-web.node/node_modules/bluebird/js/release/promise.js:512:31)
     code: 'ETIMEDOUT',
     errno: 'ETIMEDOUT',
     syscall: 'read',
     fatal: true,
     sql: 'SELECT `Id` AS `id`, `PageUrl` AS `target`, `FromUrl` AS `from`, `Type` AS `type`, `BrowserName` AS `browserName`, `BrowserVersion` AS `browserVersion`, `Ip` AS `ip`, `Title` AS `title`, `UserAgent` AS `userAgent`, `Row` AS `rowNum`, `Col` AS `colNum`, `DomType` AS `domType`, `DomId` AS `domId`, `DomClass` AS `domClass`, `DomXPosition` AS `domX`, `DomYPosition` AS `domY`, `Platform` AS `platform`, `Message` AS `msg`, `Attribute` AS `attribute`, `ApplyId` AS `applyId`, `Level` AS `level`, `Env` AS `env`, `Os` AS `os`, `Engine` AS `engine`, `AddTime` AS `addTime`, `UpdateTime` AS `updateTime` FROM `FT_Log` AS `log` ORDER BY addTime desc;' },
  sql: 'SELECT `Id` AS `id`, `PageUrl` AS `target`, `FromUrl` AS `from`, `Type` AS `type`, `BrowserName` AS `browserName`, `BrowserVersion` AS `browserVersion`, `Ip` AS `ip`, `Title` AS `title`, `UserAgent` AS `userAgent`, `Row` AS `rowNum`, `Col` AS `colNum`, `DomType` AS `domType`, `DomId` AS `domId`, `DomClass` AS `domClass`, `DomXPosition` AS `domX`, `DomYPosition` AS `domY`, `Platform` AS `platform`, `Message` AS `msg`, `Attribute` AS `attribute`, `ApplyId` AS `applyId`, `Level` AS `level`, `Env` AS `env`, `Os` AS `os`, `Engine` AS `engine`, `AddTime` AS `addTime`, `UpdateTime` AS `updateTime` FROM `FT_Log` AS `log` ORDER BY addTime desc;' }
[2017-05-26 18:14:40.444] [INFO] prism-monitor-logstore-web - 2017-05-26 18:14:40.443 10.66.66.30 -- GET /monitor/logstore/log/query 200 HTTP/1.0, 44 Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36 925672ms
appenv {"env":"test","deployenv":"qa","zkserver":"10.72.198.216:2181,10.72.193.216:2181,10.72.196.216:2181,10.72.194.216:2181,10.72.195.216:2181"}
Zookeeper connection: connecting...
Zookeeper connection: state(connected).
[2017-05-26 18:50:39.000] [DEBUG] prism-monitor-logstore-web - redis subscribe key: prism-logger-errorLog
[2017-05-26 18:53:50.602] [INFO] prism-monitor-logstore-web - Request ID:  6a577e18fad3373092008e6442982d92
[2017-05-26 18:53:50.607] [INFO] prism-monitor-logstore-web - 查询条件 {}
[2017-05-26 18:53:50.634] [TRACE] prism-monitor-logstore-web - Executing (default): SELECT `Id` AS `id`, `PageUrl` AS `target`, `FromUrl` AS `from`, `Type` AS `type`, `BrowserName` AS `browserName`, `BrowserVersion` AS `browserVersion`, `Ip` AS `ip`, `Title` AS `title`, `UserAgent` AS `userAgent`, `Row` AS `rowNum`, `Col` AS `colNum`, `DomType` AS `domType`, `DomId` AS `domId`, `DomClass` AS `domClass`, `DomXPosition` AS `domX`, `DomYPosition` AS `domY`, `Platform` AS `platform`, `Message` AS `msg`, `Attribute` AS `attribute`, `ApplyId` AS `applyId`, `Level` AS `level`, `Env` AS `env`, `Os` AS `os`, `Engine` AS `engine`, `AddTime` AS `addTime`, `UpdateTime` AS `updateTime` FROM `FT_Log` AS `log` ORDER BY addTime desc;
[2017-05-26 18:53:50.768] [INFO] prism-monitor-logstore-web - 2017-05-26 18:53:50.767 10.66.66.30 -- GET /monitor/logstore/log/query 200 HTTP/1.0, 469393 Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36 167ms
[2017-05-26 18:59:56.051] [INFO] prism-monitor-logstore-web - Request ID:  2088266ddf328fc4cf9779c4044ec960
[2017-05-26 18:59:56.056] [INFO] prism-monitor-logstore-web - 查询条件 {}
[2017-05-26 18:59:56.074] [TRACE] prism-monitor-logstore-web - Executing (default): SELECT `Id` AS `id`, `PageUrl` AS `target`, `FromUrl` AS `from`, `Type` AS `type`, `BrowserName` AS `browserName`, `BrowserVersion` AS `browserVersion`, `Ip` AS `ip`, `Title` AS `title`, `UserAgent` AS `userAgent`, `Row` AS `rowNum`, `Col` AS `colNum`, `DomType` AS `domType`, `DomId` AS `domId`, `DomClass` AS `domClass`, `DomXPosition` AS `domX`, `DomYPosition` AS `domY`, `Platform` AS `platform`, `Message` AS `msg`, `Attribute` AS `attribute`, `ApplyId` AS `applyId`, `Level` AS `level`, `Env` AS `env`, `Os` AS `os`, `Engine` AS `engine`, `AddTime` AS `addTime`, `UpdateTime` AS `updateTime` FROM `FT_Log` AS `log` ORDER BY addTime desc;
[2017-05-26 18:59:56.210] [INFO] prism-monitor-logstore-web - 2017-05-26 18:59:56.210 10.66.66.30 -- GET /monitor/logstore/log/query 200 HTTP/1.0, 469393 Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36 162ms
appenv {"env":"test","deployenv":"qa","zkserver":"10.72.198.216:2181,10.72.193.216:2181,10.72.196.216:2181,10.72.194.216:2181,10.72.195.216:2181"}
Zookeeper connection: connecting...
Zookeeper connection: state(connected).
[2017-05-26 19:26:43.366] [DEBUG] prism-monitor-logstore-web - redis subscribe key: prism-logger-errorLog
appenv {"env":"test","deployenv":"qa","zkserver":"10.72.198.216:2181,10.72.193.216:2181,10.72.196.216:2181,10.72.194.216:2181,10.72.195.216:2181"}
Zookeeper connection: connecting...
Zookeeper connection: state(connected).
[2017-05-26 19:28:57.646] [DEBUG] prism-monitor-logstore-web - redis subscribe key: prism-logger-errorLog
[2017-05-26 19:31:38.655] [INFO] prism-monitor-logstore-web - Request ID:  b2e7af24b7927cb6bd85f2fd19d5b34b
[2017-05-26 19:31:38.659] [INFO] prism-monitor-logstore-web - 查询条件 {}
[2017-05-26 19:31:38.754] [TRACE] prism-monitor-logstore-web - Executing (default): SELECT `Id` AS `id`, `PageUrl` AS `target`, `FromUrl` AS `from`, `Type` AS `type`, `BrowserName` AS `browserName`, `BrowserVersion` AS `browserVersion`, `Ip` AS `ip`, `Title` AS `title`, `UserAgent` AS `userAgent`, `Row` AS `rowNum`, `Col` AS `colNum`, `DomType` AS `domType`, `DomId` AS `domId`, `DomClass` AS `domClass`, `DomXPosition` AS `domX`, `DomYPosition` AS `domY`, `Platform` AS `platform`, `Message` AS `msg`, `Attribute` AS `attribute`, `ApplyId` AS `applyId`, `Level` AS `level`, `Env` AS `env`, `Os` AS `os`, `Engine` AS `engine`, `AddTime` AS `addTime`, `UpdateTime` AS `updateTime` FROM `FT_Log` AS `log` ORDER BY addTime desc;
[2017-05-26 19:31:38.920] [INFO] prism-monitor-logstore-web - 2017-05-26 19:31:38.919 10.66.66.30 -- GET /monitor/logstore/log/query 200 HTTP/1.0, 469393 Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36 267ms
[2017-05-26 19:45:09.535] [INFO] prism-monitor-logstore-web - Request ID:  7edfea555b7141aa44295cf2ddf22357
[2017-05-26 19:45:09.539] [INFO] prism-monitor-logstore-web - 查询条件 {}
[2017-05-26 19:45:09.570] [TRACE] prism-monitor-logstore-web - Executing (default): SELECT `Id` AS `id`, `PageUrl` AS `target`, `FromUrl` AS `from`, `Type` AS `type`, `BrowserName` AS `browserName`, `BrowserVersion` AS `browserVersion`, `Ip` AS `ip`, `Title` AS `title`, `UserAgent` AS `userAgent`, `Row` AS `rowNum`, `Col` AS `colNum`, `DomType` AS `domType`, `DomId` AS `domId`, `DomClass` AS `domClass`, `DomXPosition` AS `domX`, `DomYPosition` AS `domY`, `Platform` AS `platform`, `Message` AS `msg`, `Attribute` AS `attribute`, `ApplyId` AS `applyId`, `Level` AS `level`, `Env` AS `env`, `Os` AS `os`, `Engine` AS `engine`, `AddTime` AS `addTime`, `UpdateTime` AS `updateTime` FROM `FT_Log` AS `log` ORDER BY addTime desc;
[2017-05-26 19:45:09.707] [INFO] prism-monitor-logstore-web - 2017-05-26 19:45:09.707 10.66.67.23 -- GET /monitor/logstore/log/query 200 HTTP/1.0, 469393 Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36 173ms
packet_write_wait: Connection to 10.72.220.164 port 58422: Broken pipe
```