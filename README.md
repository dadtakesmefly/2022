# 2022

rem2px

```
  var fs = require("fs")
  var path = require("path")

  /**
   * 将指定src目录下的所有文件剪切到指定目标dest目录下
   * @param src 源目录
   * @param dest 目标目录
   */
  function copyDirectory(src, dest) {
      var files = fs.readdirSync(src)
      console.log('files', files)

      files.forEach((item) => {
          // if (item.includes('.acss')) {
          console.log('item', item)
          var itemPath = path.join(src, item)
          console.log('itemPath', itemPath)
          var itemStat = fs.statSync(itemPath) // 获取文件信息
          console.log('itemStat', itemStat)
          var savedPath = path.join(dest, itemPath.replace(src, ""))
          console.log('savedPath', savedPath)
          var savedDir = savedPath.substring(0, savedPath.lastIndexOf("\\"))
          console.log('savedDir', savedDir)
          if (itemStat.isFile()) {

              // 如果目录不存在则进行创建
              if (!fs.existsSync(savedDir)) {
                  fs.mkdirSync(savedDir, { recursive: true })
              }
              // 写入到新目录下
              var data = fs.readFileSync(itemPath)
              console.log('data', data)

              var ss = data.toString().split(";")
              console.log('ss', ss)

              for (var i = 0; i < ss.length; i++) {
                  if (/[\d.]*(rem)/.test(ss[i]) == true) {
                      var arrPx = ss[i].match(/[\d.]*(rem)/g)
                      console.log('arrPx', arrPx)
                      var remTmp = ""
                      //此循环解决多个px同一行这个情况
                      for (var j = 0; j < arrPx.length; j++) {

                          var tmp = arrPx[j].replace("rem", "") * 1
                          var rem = (tmp * 100).toFixed(0) + "px"
                          remTmp = ss[i].replace(/[\d.]*(rem)/, rem) //不可全局替换，replace不会改变ss[i]的值
                          console.log(remTmp)
                          ss[i] = remTmp //改变ss[i]
                      }
                  }
              }
              var resultStr = ss.join(";")
              fs.writeFileSync(savedPath, resultStr)

              // 并且删除原文件
              // fs.unlinkSync(itemPath)
          } else if (itemStat.isDirectory()) {
              copyDirectory(itemPath, path.join(savedDir, item))
          }
          // }

      })
      // 并且删除原目录
      // fs.rmdirSync(src)
  }


  copyDirectory("E:\\project\\demo", "E:\\project\\demo")
```

```
     // 去掉字符串首尾字符
      function remove(val, index) {
        let newVal = "" + val;
        if (index != 0) {
          return newVal.substring(1, newVal.length - 1);
        } else {
          return newVal;
        }
      }
      /* 初始化重写my.request方法,*/
          function initRequest() {
              /* 备份my.request */
              var myrequest = my.request;
              /* 更改wx.request方法为可写 */
              Object.defineProperty(my, 'request', {
                  writable: true
              });
              /* 重写my.request方法 */
              my.request = function (obj) {
                  let key = Math.random()
                  obj.key = key
                  console.log(obj)
                  /* 备份成功回调方法 */
                  var _success = obj.success;
                  /* 重写成功回调方法 */
                  obj.success = function (data) {
                      data.key = key
                      console.log('重写成功回调方法', data)
                      /* 调用备份的原成功回调方法 */
                      _success(data);
                  }
                  myrequest(obj);
              };
          }

          initRequest()
          
          
          //  
          
              (function () {
      let random;
      let sendParamsArr = [];
      let fetchParamsArr = [];
      window.__NETWORK__ = [];
      window.__CONSOLE__ = [];


      function createStyle() {
        return `*{padding:0;margin:0;border:0; touch-action: none;}.loading{width:100px;height:40px;color:white;background:#4dc86f;  position: fixed;bottom: 100px;right: 100px;  border-radius: 4px;display: flex;justify-content: center;align-items: center;}.pageContainer{}.pageContainer-hide{display:none},.pageContainer{height: 100vh;width: 100vw;background-color: rgba(0, 0, 0, 0.3);}.box{height: 75vh;width: 100%;position: fixed;bottom: 0;background-color: white;display: flex;flex-direction:column;}.tab {display: flex;height: 0.7rem;width: 100 %;border-bottom: 1px solid #333;border-top:1px solid #333}.log-panel {flex: 1;display: flex;align-items: center;justify-content:center;font-size:20px}.net-panel {flex: 1;display: flex;align-items: center;justify-content: center;font-size:20px}.log-list {flex: 1;background-color: green;width: 100%;overflow-y:auto}.mask{width:100vw;}.net-list{flex: 1;background-color: pink;width: 100%;overflow-y:auto}
        .flex {display: flex;}.flex1 {flex: 1;}.w-100 {width: 100%;border-bottom: 1px solid #333;w-33 {width: 33%;word-wrap: break-word;padding: 0 10px;box-sizing: border-box;}`
      }

      function addStyle() {
        var style = document.createElement("style");
        style.type = "text/css";
        style.innerHTML = createStyle();
        window.document.head.appendChild(style);
      }

      function addBtn() {
        var div = document.createElement("div");
        div.className = "loading";
        div.id = 'btnFS';
        div.innerText = 'TaoConsole'
        window.document.body.appendChild(div);
      }


      function addPageContainer() {
        var div = document.createElement("div");
        div.className = "pageContainer pageContainer-hide";
        div.id = 'pageContainer';
        // div.innerText = window.__CONSOLE__;

        let html = `<div class="content">
            <div class="mask">
              <div class="box">
                <div class="tab">
                  <div id='log-panel'  class="log-panel">log</div>
                  <div id='net-panel'  class="net-panel">network</div>
                </div>
                <div id='log-list' class="log-list"></div>
                <div id='net-list' class="net-list pageContainer-hide">
                   <div class="flex" >
                    <div class="flex1">Url</div>
                    <div class="flex1">params</div>
                    <div class="flex1">result</div>
                  </div>
                  <div id='net-item'>

                  </div>
                </div>
              </div>
            </div>
          </div>`

        div.appendHTML(html);

        window.document.body.appendChild(div);
      }

      function drag() {
        var flag, toggle, btnEle, cur, nx, ny, dx, dy, x, y, pageContainerEle;
        flag = false;
        toggle = false;
        btnEle = document.getElementById("btnFS");

        setTimeout(() => {
          pageContainerEle = document.getElementById("pageContainer");

        }, 1000);

        cur = { x: 0, y: 0 };

        function down() {
          var touch = event.touches[0];
          flag = true;
          cur.x = touch.clientX;
          cur.y = touch.clientY;
          dx = btnEle.offsetLeft;
          dy = btnEle.offsetTop;
        }
        function move() {
          if (flag) {
            var touch = event.touches[0];
            nx = touch.clientX - cur.x;
            ny = touch.clientY - cur.y;
            x = dx + nx;
            y = dy + ny;
            if (Math.abs(nx)) {
              event.preventDefault();
            }
            if (x <= 0) {
              x = 0;
            } else if (x >= btnEle.parentNode.offsetWidth - btnEle.offsetWidth) {
              x = btnEle.parentNode.offsetWidth - btnEle.offsetWidth;
            } else {
              x = x;
            }

            if (y <= 0) {
              y = 0;
            } else if (y >= btnEle.parentNode.offsetHeight - btnEle.offsetHeight) {
              y = btnEle.parentNode.offsetHeight - btnEle.offsetHeight;
            } else {
              y = y;
            }
            btnEle.style.left = x + "px";
            btnEle.style.top = y + "px";
          }
        }
        function end() {
          flag = false;
        }
        btnEle.addEventListener("touchstart", function () {
          down();
        }, false);
        btnEle.addEventListener("touchmove", function () {
          move();
        }, false);
        btnEle.addEventListener("touchend", function () {
          end();
        }, false);
        btnEle.addEventListener("click", function (event) {
          toggle = !toggle

          if (toggle) {
            pageContainerEle.classList.remove('pageContainer-hide')
          } else {
            pageContainerEle.classList.add('pageContainer-hide')
          }
        });
      }



      window.onload = function () {
        addStyle();
        addBtn();
        drag();
        addPageContainer();
        console.log('hello TaoConsloe');

        setTimeout(() => {
          document.getElementById('log-panel') && document.getElementById('log-panel').addEventListener('click', function (e) {
            var netListEle = document.getElementById('net-list');
            var logListEle = document.getElementById('log-list');
            netListEle.classList.add('pageContainer-hide')
            logListEle.classList.remove('pageContainer-hide')
          })

          document.getElementById('net-panel') && document.getElementById('net-panel').addEventListener('click', function (e) {
            var netListEle = document.getElementById('net-list');
            var logListEle = document.getElementById('log-list');
            logListEle.classList.add('pageContainer-hide')
            netListEle.classList.remove('pageContainer-hide')
          })
        }, 1000);

      }

      HTMLElement.prototype.appendHTML = function (html) {
        var divTemp = document.createElement("div"), nodes = null
          // 文档片段，一次性append，提高性能
          , fragment = document.createDocumentFragment();
        divTemp.innerHTML = html;
        nodes = divTemp.childNodes;
        for (var i = 0, length = nodes.length; i < length; i += 1) {
          fragment.appendChild(nodes[i].cloneNode(true));
        }
        this.appendChild(fragment);
        // 据说下面这样子世界会更清净
        nodes = null;
        fragment = null;
      };

      // console.log 重写

      console.log = (function (oriLogFunc) {
        return function () {
          try {
            oriLogFunc.call(console, ...arguments);
            window.__CONSOLE__.push(...arguments)
            if (document.getElementById('log-list')) {
              document.getElementById('log-list').innerHTML = '';
            }

            let html = window.__CONSOLE__.map((n) => {
              return (`<div class='w-100'>${JSON.stringify(n)}</div>`)
            }).join('')

            document.getElementById('log-list') && document.getElementById('log-list').appendHTML(html)

          } catch (e) {
            console.error('console.log error', e);
          }
        }
      })(console.log);


      //  XMLHttpRequest 重写
      var originOpen = XMLHttpRequest.prototype.open;
      XMLHttpRequest.prototype.open = function (method, url, async, user, pass) {
        random = Math.random();
        this.key = random;
        this.addEventListener('load', function () {
          let obj = {}
          obj.url = this.responseURL.includes('@') ? this.responseURL.split('@')[1] : this.responseURL
          obj.res = JSON.parse(this.responseText);
          sendParamsArr.forEach((n, i) => {
            if (n[1] === this.key) {
              obj.params = n[0]
            }
          })
          window.__NETWORK__.push(obj);

          if (document.getElementById('net-item')) {
            document.getElementById('net-item').innerHTML = '';

          }

          let html = window.__NETWORK__.map((n) => {
            return (`
                  <div key={i} class="flex w-100">
                    <div class="w-33">${n.url}</div>
                    <div class="w-33">${JSON.stringify(n.params)}</div>
                    <div class="w-33">${JSON.stringify(n.res)}</div>
                  </div>
            `)
          }).join('')

          document.getElementById('net-item') && document.getElementById('net-item').appendHTML(html)

        });

        let args = [...arguments, random]
        originOpen.apply(this, args);
      };

      var originSend = XMLHttpRequest.prototype.send;
      XMLHttpRequest.prototype.send = function () {
        let args = [...arguments, random]
        sendParamsArr.push(args)
        originSend.apply(this, args)
      }


      const originFetch = fetch;

      window.fetch = (url, options) => {
        let key = Math.random();
        let obj = {};
        obj.key = key;
        obj.url = url;
        obj.params = options;
        fetchParamsArr.push(obj)
        return originFetch(url, options).then(async (response) => {
          response.key = key;
          obj.res = await response.json();
          fetchParamsArr.forEach((n, i) => {
            if (response.key === n.key) {
              obj.params = n.params
            }
          })
          window.__NETWORK__.push(obj);
          return obj.res;
        }).catch(err => {
          console.log(err)
        })
      };

    })()
      
