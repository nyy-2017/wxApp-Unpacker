# wxApp-Unpacker
最新‘微信小程序’反编译最新教程，如何找回微信小程序源码手把手教程！！！
2019年8月15日；新增了反编译css样式丢失问题；（已经做好的见zip文件）：wxApp-Unpacker-master.zip
反编译微信小程序错误: $gwx is not defined和__vd_version_info__ is not defined 已解决
修改wxappUnpacker文件中的 wuWxss.js
function runVM(name, code) {
      // let wxAppCode = {}, handle = {cssFile: name};
      // let vm = new VM({
      //    sandbox: Object.assign(new GwxCfg(), {
      //       __wxAppCode__: wxAppCode,
      //       setCssToHead: cssRebuild.bind(handle)
      //    })
      // });
      // vm.run(code);
      // for (let name in wxAppCode) if (name.endsWith(".wxss")) {
      //    handle.cssFile = path.resolve(frameName, "..", name);
      //    wxAppCode[name]();
      // }
 
      let wxAppCode = {};
      let handle = {cssFile: name};
      let gg = new GwxCfg();
      let tsandbox = {
         $gwx: GwxCfg.prototype["$gwx"],
         __mainPageFrameReady__: GwxCfg.prototype["$gwx"],   //解决 $gwx is not defined
         __vd_version_info__: GwxCfg.prototype["$gwx"],  //解决 __vd_version_info__ is not defined
         __wxAppCode__: wxAppCode,
         setCssToHead: cssRebuild.bind(handle)
      }
 
      let vm = new VM({sandbox: tsandbox});
      vm.run(code);
      for (let name in wxAppCode) {
         if (name.endsWith(".wxss")) {
            handle.cssFile = path.resolve(frameName, "..", name);
            wxAppCode[name]();
         }
      }
   }
