# Excel 操作

 

> 使用阿里开源的 EasyExcel 

pom导入依赖



~~~xml
<dependencies>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>easyexcel</artifactId>
        <version>2.2.6</version>
    </dependency>
</dependencies>
~~~



## 读取数据

~~~java
package com.southwind.test;

import com.alibaba.excel.EasyExcel;
import com.alibaba.excel.ExcelReader;
import com.alibaba.excel.context.AnalysisContext;
import com.alibaba.excel.event.AnalysisEventListener;
import com.alibaba.excel.read.builder.ExcelReaderBuilder;
import com.alibaba.excel.support.ExcelTypeEnum;

public class Test {
    public static void main(String[] args) {
        // 读取文件
        // 创建ExcelReaderBuilder 实例
        ExcelReaderBuilder readerBuilder = EasyExcel.read();

        // 获取文件对象
        readerBuilder.file("D:\\excel\\用户数据表.xls");

        // 指定sheet (不指定默认指定所有sheet)
        readerBuilder.sheet("用户数据表");

        // 自动关闭输入流
        readerBuilder.autoCloseStream(true);

        // 设置Excel文件格式
        readerBuilder.excelType(ExcelTypeEnum.XLS);

        // 注册监听器，进行数据的解析
        readerBuilder.registerReadListener(new AnalysisEventListener() {
            @Override
            public void invoke(Object o, AnalysisContext analysisContext) {
                // 一行数据读取完成之后的回调
                System.out.println(o);

            }

            @Override
            public void doAfterAllAnalysed(AnalysisContext analysisContext) {
                // 通知文件读取完毕
                System.out.println("数据读取完毕");
            }
        });

        // 构建读取器
        ExcelReader reader = readerBuilder.build();
        // 读取数据 
        reader.readAll();
        // 读取完毕
        reader.finish();
    }
}
~~~

![easyExcel](D:\typora\JAVA-MD\easyExcel.png)



读取操作完成。



可以通过泛型指定数据类型， Map集合



~~~java
package com.southwind.test;

import com.alibaba.excel.EasyExcel;
import com.alibaba.excel.ExcelReader;
import com.alibaba.excel.context.AnalysisContext;
import com.alibaba.excel.event.AnalysisEventListener;
import com.alibaba.excel.read.builder.ExcelReaderBuilder;
import com.alibaba.excel.support.ExcelTypeEnum;

import java.util.Iterator;
import java.util.Map;
import java.util.Set;

public class Test {
    public static void main(String[] args) {
        // 读取文件
        // 创建ExcelReaderBuilder 实例
        ExcelReaderBuilder readerBuilder = EasyExcel.read();

        // 获取文件对象
        readerBuilder.file("D:\\excel\\用户数据表.xls");

        // 指定sheet (不指定默认指定所有sheet)
        readerBuilder.sheet("用户数据表");

        // 自动关闭输入流
        readerBuilder.autoCloseStream(true);

        // 设置Excel文件格式
        readerBuilder.excelType(ExcelTypeEnum.XLS);

        // 注册监听器，进行数据的解析
        readerBuilder.registerReadListener(new AnalysisEventListener<Map<Integer,String>>() {
            @Override
            public void invoke(Map<Integer, String> integerStringMap, AnalysisContext analysisContext) {
                // 先获取Key值
                Set<Integer> keySet = integerStringMap.keySet();
                Iterator<Integer> iterator = keySet.iterator();
                while (iterator.hasNext()){
                    Integer key = iterator.next();
                    System.out.println(key + ":" + integerStringMap.get(key)+ ", ");
                }
                System.out.println("");
            }

            @Override
            public void doAfterAllAnalysed(AnalysisContext analysisContext) {
                // 通知文件读取完毕
                System.out.println("数据读取完毕");
            }
        });

        // 构建读取器
        ExcelReader reader = readerBuilder.build();
        // 读取数据
        reader.readAll();
        // 读取完毕
        reader.finish();

    }
}
~~~



实际开发中会对上述代码进行简化 。

~~~java
package com.southwind.test;

import com.alibaba.excel.EasyExcel;
import com.alibaba.excel.context.AnalysisContext;
import com.alibaba.excel.event.AnalysisEventListener;
import com.alibaba.excel.read.builder.ExcelReaderBuilder;

import java.util.*;

public class ExcelText {
    public static void main(String[] args) {

        List<Map<Integer,String>> list = new LinkedList<>();

        EasyExcel.read("D:\\excel\\用户数据表.xls")
                .sheet()
                .registerReadListener(new AnalysisEventListener<Map<Integer,String>>() {

                    @Override
                    public void invoke(Map<Integer, String> integerStringMap, AnalysisContext analysisContext) {
                        list.add(integerStringMap);

                    }

                    @Override
                    public void doAfterAllAnalysed(AnalysisContext analysisContext) {
                        System.out.println("数据读取完毕");
                    }
                }).doRead();
        for (Map<Integer, String> integerStringMap : list) {
            Set<Integer> keySet = integerStringMap.keySet();
            Iterator<Integer> iterator = keySet.iterator();
            while(iterator.hasNext()){
                Integer key = iterator.next();
                System.out.println(key + ":" + integerStringMap.get(key)+", ");

            }
            System.out.println("");
        }

    }

}

~~~



映射成为 指定对象，需要创建实体类 @ExcelProperty 注解完成实体类成员变量 和 Excel 字段的 映射

~~~java
package com.southwind.entity;

import com.alibaba.excel.annotation.ExcelProperty;
import lombok.Data;

import java.util.Date;

@Data
public class ExcelData {

    @ExcelProperty("ID")
    private String id;
    @ExcelProperty("用户名")
    private String name;
    @ExcelProperty("邮箱")
    private String email;
    @ExcelProperty("性别")
    private String gender;
    @ExcelProperty("积分")
    private Integer score;
    @ExcelProperty("IP")
    private String ip;
    @ExcelProperty("登入次数")
    private Integer count;
    @ExcelProperty("加入时间")
    private Date date;
}
~~~



解析Excel 文件的时候 ，直接指定实体类 即可





~~~java
package com.southwind.test;

import com.alibaba.excel.EasyExcel;
import com.alibaba.excel.context.AnalysisContext;
import com.alibaba.excel.event.AnalysisEventListener;
import com.alibaba.excel.read.builder.ExcelReaderBuilder;
import com.southwind.entity.ExcelData;

import java.util.*;

public class ExcelText {
    public static void main(String[] args) {

        List<ExcelData> list = new LinkedList<>();

        EasyExcel.read("D:\\excel\\用户数据表.xls")
                .head(ExcelData.class)
                .sheet()
                .registerReadListener(new AnalysisEventListener<ExcelData>() {
                    
                    @Override
                    public void invoke(ExcelData excelData, AnalysisContext analysisContext) {
                        list.add(excelData);
                    }

                    @Override
                    public void doAfterAllAnalysed(AnalysisContext analysisContext) {
                        System.out.println("数据读取完毕");
                    }
                }).doRead();
        for (ExcelData excelData : list) {
            System.out.println(excelData);
        }
    }

}
~~~





## 写入数据

~~~java
package com.southwind.test;

import com.alibaba.excel.EasyExcel;
import com.alibaba.excel.context.AnalysisContext;
import com.alibaba.excel.event.AnalysisEventListener;
import com.alibaba.excel.read.builder.ExcelReaderBuilder;
import com.alibaba.excel.support.ExcelTypeEnum;
import com.southwind.entity.ExcelData;

import java.util.*;

public class ExcelText {
    public static void main(String[] args) {
        List<ExcelData> list = parseData();

        // 把list 写入 Excel 文件
        EasyExcel.write("D:\\excel\\用户数据表--副本.xls")
                .head(ExcelData.class)
                .excelType(ExcelTypeEnum.XLS)
                .sheet("用户数据表")
                .doWrite(list);
    }

    public static List<ExcelData> parseData() {
        List<ExcelData> list = new LinkedList<>();

        EasyExcel.read("D:\\excel\\用户数据表.xls")
                .head(ExcelData.class)
                .sheet()
                .registerReadListener(new AnalysisEventListener<ExcelData>() {

                    @Override
                    public void invoke(ExcelData excelData, AnalysisContext analysisContext) {
                        list.add(excelData);
                    }

                    @Override
                    public void doAfterAllAnalysed(AnalysisContext analysisContext) {
                        System.out.println("数据读取完毕");
                    }
                }).doRead();
        return list;
    }
}
~~~























