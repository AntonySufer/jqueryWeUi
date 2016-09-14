# jqueryWeUi
适用微信的组件，上拉刷新，下拉刷新，滚动，选择器，日历，城市
1xia'l 下拉 shua'xdeng刷新


   <div class="" >
    <div class=" weui-pull-to-refresh-layer scroll_user " >
   <div class="pull-to-refresh-arrow"></div> <!-- 上下拉动的时候显示的箭头 -->
      <div class="pull-to-refresh-preloader"></div> <!-- 正在刷新的菊花 -->
      <div class="down">下拉刷新</div><!-- 下拉过程显示的文案 -->
      <div class="up">释放刷新</div><!-- 下拉超过50px显示的文案 -->
      <div class="refresh">正在刷新...</div><!-- 正在刷新时显示的文案 -->
    </div>
    <%--若关联--%>
    <div class="shop_list_box inner"  id="thelist" >

     </div>  
     <!-- 下拉刷新 -->
     <div class="weui-infinite-scroll scroll_user" id="downScroll" >
      <div class="infinite-preloader"></div>
                      正在加载
    </div>
  </div>
<script>
    $(function(){

       FastClick.attach(document.body);
    // 按钮操作
       getShopList();
     $(document.body).pullToRefresh().on("pull-to-refresh", function() {
         //下拉刷新
        getShopList('html');

      });
      // infinite
      var loading = false;
      $(document.body).infinite().on("infinite", function() {
        //上啦加载
        if(loading){
           return;
        }
        getShopList('append');
      });
      
     /***
      * 获取店铺列表
      */ 
     function getShopList(type){
         $.fetchAjaxData({url:"http://wx.mljiadev.cn/mp/user/openid//ox1dUs33LfD9kwxjcCUBOoIz4Krs/shops?token=MTQ3MzgxOTQ4OTM1OW94MWRVczMzTGZEOWt3eGpjQ1VCT29JejRLcnN3eDlmMjg0MDE5YWNkNzViNGEjKmg1YXV0aCoj&offset=1&limit=30"},
         function(jsonData){
              if (jsonData.returnCode =="success") {
                var shopList =jsonData.shops;
                var html =　"";
            for (var i = 0; i < shopList.length; i++) {
                
               shopList[i].shopLogo =GLOBAL.mpDownLoadImgUrl+globalUtil.checkImgIsNumber(shopList[i].shopLogo);
                html +=" <div class='shop_box' id=''>"
                    +" <div class='shop_box_center'>"
                    +"     <div class='shop_box_pic'>"
                    +"  <img  alt='' src="+ shopList[i].shopLogo+">"
                    +"    </div>"
                    +"    <div class='shop_box_inf'>"
                    +"       <div class='shop_inf_title'>"
                    +"         <span class='shop_inf_title_p1 hid_slh1' v-text='item.shopName'>店铺名称"+i+"</span>"
                    +"           <a href='javascript:;' class='btn_shop_inf_kx' >卡项</a> "
                    +"        </div>"
                    +"        <div class='shop_inf_lv' >"
                    +"        </div>"
                    +"        <p class='shop_inf_add hid_slh2' ></p>"
                    +"    </div>"
                    +" </div>"
                    +"  </div>";
                }
                    if (type=="append") {
                       $("#thelist").append(html);
                    }else{
                         $("#thelist").html(html+html);
                         var screenHig = window.innerHeight ;//手机屏幕高度
                         var scrollHig =  $("#thelist").height();//内容高度
                         if (scrollHig>screenHig) {
                          $("#downScroll").show();//加载显示
                          $(document.body).pullToRefreshDone();
                         }else{
                            //不足一页
                          $("#downScroll").hide();//加载隐藏
                         }
                         
                         
                     }
                  
                  }
         });
 
        }
});

</script>

