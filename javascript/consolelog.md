# console.log

- cosole 차단
````
Object.defineProperty(console, '_commandLineAPI', { get : function() { throw 'No!' } })
````
- console 더미 객체
````
if(!window.console) { (function(win){ var names = [ 'assert', 'clear', 'count', 'debug', 'dir', 'dirxml', 'error', 'exception', 'group', 'groupCollapsed', 'groupEnd', 'info', 'log', 'markTimeline', 'profile', 'profileEnd', 'table', 'time', 'timeEnd', 'timeline', 'timelineEnd', 'timeStamp', 'trace', 'warn' ]; var consoleMock = {}; for (var i = 0, len = names.length; i < len; i++) { consoleMock[names[i]] = function () {}; } win.console = consoleMock; })(window); }
````
https://stackoverflow.com/questions/21692646/how-does-facebook-disable-the-browsers-integrated-developer-tools/21693931#21693931

http://webclub.tistory.com/464