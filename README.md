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

