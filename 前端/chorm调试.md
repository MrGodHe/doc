chorm 谷歌浏览器 调试： sources  >> snippet

```
var i = 1;
[].map((index)=> {      
    setTimeout(function(){return download(index);}, i*1000);
     i = i+10;
    return index;
})

function download(orgId) {
     console.info("[机构："+ orgId + "]时间：" + new Date().Format("yyyy-MM-dd hh:mm:ss"));
     try{

        var form = $("<form>"); //定义一个form表单
        form.attr('style', 'display:none'); //在form表单中添加查询参数
        form.attr('target',  '');//默认父窗口
        form.attr('method', 'POST');
        form.attr('action', '');
        $('body').append(form); 

    console.info(form);

    var input1 = $('<input>');
        input1.attr('type', 'hidden');
        input1.attr('name', 'orgId');
        input1.attr('value', orgId);
        form.append(input1);

     var input2 = $('<input>');
        input2.attr('type', 'hidden');
        input2.attr('name', 'start');
        input2.attr('value', '2018-01-01');
        form.append(input2);

         var input3 = $('<input>');
        input3.attr('type', 'hidden');
        input3.attr('name', 'end');
        input3.attr('value', '2023-06-05');
        form.append(input3);
    
    form.submit().remove();
           
    }catch(err){
        
    }
    
    
}


```

