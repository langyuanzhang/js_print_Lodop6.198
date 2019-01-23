# js_print_Lodop6.198
在js中使用Lodop打印


Html界面
```html
<div id="textarea01" style="width: 300px;margin: auto;border: 1px #000 solid;display: none">
    <div style="width: 90%;margin: auto;padding: 30px 0;">
        <h3 style="text-align: center;padding-top:20px;font-size: 20px;">配送订单</h3>
        <h3 style="text-align: center;padding-top:10px;font-size: 16px;">小程序订单</h3>
        <div style="font-size: 16px; padding: 10px 0;">订单号: <span id="tai">2018010101</span></div>
        <div style="font-size: 16px; padding-bottom:10px;">收货人: <span id="username">默认</span></div>
        <div style="font-size: 16px; padding-bottom:10px;">联系电话: <span id="time">1234567890</span></div>
        <div style="border: 1px #000 solid;"></div>
        <div style="font-size: 16px; padding: 5px 0;">商品名称</div>
            <div style="width: 100%;" id="append">
                 <div style="width: 100%;line-height: 24px;padding-bottom: 5px;">
                     <span style="font-size: 20px;">餐具</span>  
                     <span style="font-size: 16px;margin-left: 10px;">2套</span>
                     <span style="font-size: 16px;margin-left: 10px;">单价2元</span>
                 </div>
            </div>
        <div style="border: 1px #000 solid;clear:both "></div>
        <div style="font-size: 18px; padding: 10px 0;">合计积分: <span id="amount">4.00</span></div>
    </div>
</div>
```

Js
```js
<script type="text/javascript" src="__common__/js/LodopFuncs.js"></script>
```
```js
// *************************点击打印按钮**********************/
function print(id){
    $.ajax({
        url: '{:url("admin/order/deliveryPrint")}',
        type: 'get',
        dataType: 'json',
        data:{id:id},
        success:function(res){
            console.log("打印",res);
            if(res != ''){
                $('#tai').html(res.order_id);
                $('#username').html(res.realname);
                $('#time').html(res.phone);
                $('#amount').html(res.price);
                $('#append').html("");
                for (var i = 0;i<res.order_details.length;  i++) {
                    $('#append').append("<div style='width: 100%;line-height: 24px;padding-bottom: 5px;'><span style='font-size: 20px;'>"+res.order_details[i].goods_name+"</span><span style='font-size: 16px;margin-left: 10px;'>"+'*'+res.order_details[i].number+"</span><span style='font-size: 16px;margin-left: 10px;'>单价"+res.order_details[i].price+"元/"+res.order_details[i].norm+"</span></div>");
                }
                issprint();
            }else{
                // toastr.error("非打印组不可打印!");
                console.log("不可打印")
            }
        }
    })
}

var LODOP; //声明为全局变量
function issprint() { 
    var html=document.getElementById("textarea01").value;
    LODOP = getLodop();
    LODOP.PRINT_INIT("");
    LODOP.SET_PRINT_STYLE("FontSize", 4); //字体大小
    LODOP.SET_PRINT_PAGESIZE(3, 780, 50, "");//这里3表示纵向打印且纸高“按内容的高度”；1385表示纸宽138.5mm；45表示页底空白4.5mm
    LODOP.ADD_PRINT_HTM(5, 8, "95%", "100%", document.getElementById("textarea01").innerHTML);
    LODOP.PRINT();
};
```
