---
layout: post
title:  "how to implement a mulit-level select group"
date:   2016-11-09 09:51:43 +0800
categories: js
---
To record how to implement a multi level select group

#### Utilize the idea of infinity classification. First let's declare the data structure.

- id: index of the data
- title: text of the data (shown in select->option text)
- pid: parent id of the data
- code: value of the data
- level: level of the select object

> Below shows the data declared in project:

```
var insurance_type_source = [
        {"id":1, "title":"人身保险", "pid": 0, "code":"1000000", "level":1},
        {"id":2, "title":"财产险", "pid": 0, "code":"2000000", "level":1},

        {"id":3, "title":"人寿保险", "pid": 1, "code":"1100000", "level":2},
        {"id":4, "title":"年金保险", "pid": 1, "code":"1200000", "level":2},
        {"id":5, "title":"健康保险", "pid": 1, "code":"1300000", "level":2},
        {"id":6, "title":"意外伤害保险", "pid": 1, "code":"1400000", "level":2},

        {"id":7, "title":"车险", "pid": 2, "code":"2100000", "level":2},
        {"id":8, "title":"家庭财产险", "pid": 2, "code":"2200000", "level":2},
        {"id":9, "title":"企业财产险", "pid": 2, "code":"2300000", "level":2},
        {"id":10, "title":"工程保险", "pid": 2, "code":"2400000", "level":2},
        {"id":11, "title":"船舶保险", "pid": 2, "code":"2500000", "level":2},
        {"id":12, "title":"农业保险", "pid": 2, "code":"2600000", "level":2},
        {"id":13, "title":"货物运输保险", "pid": 2, "code":"2700000", "level":2},
        {"id":14, "title":"责任保险", "pid": 2, "code":"2800000", "level":2},
        {"id":15, "title":"信用保险", "pid": 2, "code":"2900000", "level":2},
        {"id":16, "title":"保证保险", "pid": 2, "code":"2A00000", "level":2},
        {"id":17, "title":"综合保险", "pid": 2, "code":"2B00000", "level":2},
        {"id":18, "title":"特殊风险保险", "pid": 2, "code":"2C00000", "level":2},
        {"id":19, "title":"其他", "pid": 2, "code":"2D00000", "level":2},

        {"id":20, "title":"定期寿险", "pid": 3, "code":"1110000", "level":3},
        {"id":21, "title":"终身寿险", "pid": 3, "code":"1120000", "level":3},
        {"id":22, "title":"两全保险", "pid": 3, "code":"1130000", "level":3},

        {"id":23, "title":"一般年金保险", "pid": 4, "code":"1210000", "level":3},
        {"id":24, "title":"养老年金保险", "pid": 4, "code":"1220000", "level":3},

        {"id":25, "title":"疾病保险", "pid": 5, "code":"1310000", "level":3},
        {"id":26, "title":"医疗保险", "pid": 5, "code":"1320000", "level":3},
        {"id":27, "title":"失能收入损失保险", "pid": 5, "code":"1330000", "level":3},


        {"id":28, "title":"一般意外伤害保险", "pid": 6, "code":"1410000", "level":3},
        {"id":29, "title":"交通工具意外伤害保险", "pid": 6, "code":"1420000", "level":3},

        {"id":30, "title":"雇主责任保险", "pid": 14, "code":"2810000", "level":3},
        {"id":31, "title":"公众责任保险", "pid": 14, "code":"2820000", "level":3},
        {"id":32, "title":"产品责任险", "pid": 14, "code":"2830000", "level":3},
        {"id":33, "title":"第三者责任险", "pid": 14, "code":"2840000", "level":3},
        {"id":34, "title":"职业责任险", "pid": 14, "code":"2850000", "level":3},

        {"id":35, "title":"疾病身故定期寿险", "pid": 20, "code":"1111000", "level":4},
        {"id":36, "title":"意外身故定期寿险", "pid": 20, "code":"1112000", "level":4},
        {"id":37, "title":"一般定期寿险", "pid": 20, "code":"1113000", "level":4},

        {"id":38, "title":"重大疾病保险", "pid": 25, "code":"1311000", "level":4},
        {"id":39, "title":"特定疾病保险", "pid": 25, "code":"1312000", "level":4},

        {"id":40, "title":"费用补偿医疗保险", "pid": 26, "code":"1321000", "level":4},
        {"id":41, "title":"定额给付医疗保险", "pid": 26, "code":"1322000", "level":4},

        {"id":42, "title":"综合医疗保险", "pid": 40, "code":"1321100", "level":5},
        {"id":43, "title":"补充医疗保险", "pid": 40, "code":"1321200", "level":5},
        {"id":44, "title":"意外伤害医疗保险", "pid": 40, "code":"1321300", "level":5},
        {"id":45, "title":"公共交通意外伤害医疗", "pid": 40, "code":"1321400", "level":5},
        {"id":46, "title":"急性病医疗保险", "pid": 40, "code":"1321500", "level":5},
        {"id":47, "title":"女性生育保险", "pid": 40, "code":"1321600", "level":5},
        {"id":48, "title":"子女医疗保险", "pid": 40, "code":"1321700", "level":5},
        {"id":49, "title":"中高端医疗保险", "pid": 40, "code":"1321800", "level":5}
        //...
```

#### Then to implement the code using recursion

```
var MAX_LEVEL = 5; // to define the maximum level of the select group

// to group source data by level
function groupDataByLevel(sourceDta)
{
    var count = sourceData.length;
    var groupedTypeData  = [];
    for(var i = 0; i<count; i ++){
        if(groupedTypeData[sourceData[i].level] == undefined){
            groupedTypeData[sourceData[i].level] = [];
        }
        groupedTypeData[sourceData[i].level].push(sourceData[i]);
    }
    return groupedTypeData;
}

// to get sub select source data by parent id
function getSubDataBySelectId(parentId, sourceData){
    var parentIdData = [];
    if(parentId>0){
        var sourceCount = sourceData.length;
        for(var i = parentId; i < sourceCount; i++){
            if(sourceData[i].pid == parentId){
                parentIdData.push(sourceData[i]);
            }
        }
    }
    return parentIdData;
}

// to set current select's content
function setSelectContent(obj, typeData){
    var toAddOptions = "<option value='-1'>请选择</option>";
    for(var i = 0; i < typeData.length; i++){
        toAddOptions += "<option value='"+typeData[i].id+"' data-code='"+typeData[i].code+"'>"+typeData[i].title+"</option>";
    }
    obj.html(toAddOptions);
}


//this is the main function
//use recusion to set current select and register it's change event.
//in it's change event, it will trigger this function again to set it's child select obj's content and change event
function setSelectAndSubObjAndEvent(obj,typeData){
    setSelectContent(obj, typeData);
    var currentLevel = typeData[0].level;
    if(!obj.onchange){
        obj.on('change',function() {
            var id = $(this).val();
            var subTypeData = getSubDataBySelectId(id, insurance_type_source);
            if (subTypeData.length > 0) {
                var subLevel = subTypeData[0].level;
                setSelectAndSubObjAndEvent($('#s' + subLevel), subTypeData);
            }else{
                for(var i = currentLevel + 1; i <= MAX_LEVEL; i++){
                    setSelectContent($('#s' + i), []);
                }
            }
        });
    }
}

// initialize function
function initializeProductSelect(sourceData){
    var groupedData = groupDataByLevel(sourceData);
    setSelectAndSubObjAndEvent($('#s1'),groupedData[1]);
}

// call initialize function with source data
initializeProductSelect(insurance_type_source);

```
Actually, you can also read the maximum level from the soure data, and then you can write the infinity select level group according to the soure data!
[Click here to see the demo ](../../../../2016/11/10/multi-level_select_group_demo.html)
