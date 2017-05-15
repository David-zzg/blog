node.js 中的child_process有一个exec方法，可以调用shell脚本。今天发现exec没有执行回调方法。后来在[网上查阅资料](https://segmentfault.com/q/1010000006959963)，发现exec的输出有大小限制，当输出数据量过大时，系统会杀死进程，因而不会触发回调。  
解决方法：

    var workerProcess = child_process.exec('node node_modules/webpack/bin/webpack.js', {})
    
    workerProcess.stdout.on('data', function (data) {
        console.log('stdout: ' + data);
    });
    
    workerProcess.stderr.on('data', function (data) {
        console.log('stderr: ' + data);
    });

其实就是增加监听事件。