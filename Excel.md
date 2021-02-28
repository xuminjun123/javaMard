#  EasyExcel 操作

 

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



![easyExcel](D:\typora\JAVA-MD\ExcelImage\easyExcel.png)



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



映射成为 指定对象，需要创建实体类 `@ExcelProperty` 注解完成实体类成员变量 和 Excel 字段的 映射

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









# Apache POI 

Apache POI  官网 ：https://pol.apache.org/   会比较麻烦

Apache POI 是 提供API给Java 程序对 Office格式档案写的功能 。



## POI 写数据‘

- pom 导包

```xml
<dependencies>
    <dependency>
        <!--旧版本 xls版-->
        <groupId>org.apache.poi</groupId>
        <artifactId>poi</artifactId>
        <version>3.9</version>
    </dependency>
    <dependency>
         <!--新版本 xlxs版-->
        <groupId>org.apache.poi</groupId>
        <artifactId>poi-ooxml</artifactId>
        <version>3.9</version>
    </dependency>

    <!--如期格式化工具-->
    <dependency>
        <groupId>joda-time</groupId>
        <artifactId>joda-time</artifactId>
        <version>2.10.1</version>
    </dependency>

    <!--test-->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```



![po'l](D:\typora\JAVA-MD\ExcelImage\po'l.png)



对象有：

- 工作簿
- 工作表
- 行
- 列



旧版本：

~~~java
package com.kuang;

import org.apache.poi.hssf.usermodel.HSSFWorkbook;
import org.apache.poi.ss.usermodel.Cell;
import org.apache.poi.ss.usermodel.Row;
import org.apache.poi.ss.usermodel.Sheet;
import org.apache.poi.ss.usermodel.Workbook;
import org.joda.time.DateTime;
import org.junit.Test;

import java.io.FileNotFoundException;
import java.io.FileOutputStream;

public class ExcelWriteTest {

    String PATH = "D:\\Images\\";
    
    @Test
    public void testWrite03() throws Exception {

        // 创建一个工作簿
        Workbook workbook = new HSSFWorkbook();

        // 创建一个工作表
        Sheet sheet = workbook.createSheet("旧版本excel表");

        // 创建一个行
        Row row1 = sheet.createRow(0);

        // 创建一个单元格
        Cell cell1 = row1.createCell(0);

        // 写入数据
        cell1.setCellValue("1");
        // (1,2)
        Cell cell2 = row1.createCell(1);
        cell2.setCellValue(666);


        // 第二行
        Row row2 = sheet.createRow(1);
        Cell cell21 = row2.createCell(0);
        cell21.setCellValue("统计时间");
        // (2,2)
        Cell cell22 = row2.createCell(1);
        String time = new DateTime().toString("yyyy-MM-dd HH:mm:ss");
        cell22.setCellValue(time);

        // 生产一张表(IO 流) 旧版本使用 xls 格式
        FileOutputStream fileOutputStream = new FileOutputStream(PATH + "旧版本.xls");
        // 输出
        workbook.write(fileOutputStream);

        // 关闭流
        fileOutputStream.close();

        System.out.println("文件生成完毕");

    }

}
~~~



新版本xlsx

~~~
@Test
    public void testWrite07() throws Exception {

        // 创建一个工作簿
        Workbook workbook = new XSSFWorkbook();

        // 创建一个工作表
        Sheet sheet = workbook.createSheet("新版本excel表");

        // 创建一个行
        Row row1 = sheet.createRow(0);

        // 创建一个单元格
        Cell cell1 = row1.createCell(0);

        // 写入数据
        cell1.setCellValue("1");
        // (1,2)
        Cell cell2 = row1.createCell(1);
        cell2.setCellValue(666);


        // 第二行
        Row row2 = sheet.createRow(1);
        Cell cell21 = row2.createCell(0);
        cell21.setCellValue("统计时间");
        // (2,2)
        Cell cell22 = row2.createCell(1);
        String time = new DateTime().toString("yyyy-MM-dd HH:mm:ss");
        cell22.setCellValue(time);

        // 生产一张表(IO 流) 旧版本使用 xls 格式
        FileOutputStream fileOutputStream = new FileOutputStream(PATH + "新版本.xlsx");
        // 输出
        workbook.write(fileOutputStream);

        // 关闭流
        fileOutputStream.close();

        System.out.println("文件生成完毕");


    }

~~~



**批量导入数据**

> 大文件写 HSSF

旧版本 xls 格式：最多处理65535 行数据，否则会抛出异常

优点 ： 过程中写入缓存 ，不操作磁盘，最后一次性写入磁盘，速度快；



> 大文件XSSF



新版本 ：xlsxs 格式： 写数据书速的非常慢，非常耗内存 





## POI读取数据



新版本

~~~java
package com.kuang;

import org.apache.poi.hssf.usermodel.HSSFWorkbook;
import org.apache.poi.ss.usermodel.Cell;
import org.apache.poi.ss.usermodel.Row;
import org.apache.poi.ss.usermodel.Sheet;
import org.apache.poi.ss.usermodel.Workbook;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;
import org.joda.time.DateTime;
import org.junit.Test;

import java.io.FileInputStream;
import java.io.FileOutputStream;

public class ExcelReadTest {

    String PATH = "D:\\excel\\";

    @Test
    public void testWrite07() throws Exception {

        // 获取文件流
        FileInputStream inputStream = new FileInputStream(PATH + "新版本.xlsx");

        // 创建一个工作簿
        Workbook workbook = new XSSFWorkbook(inputStream);

        // 得到表
        Sheet sheet = workbook.getSheetAt(0);
        // 得到行
        Row row = sheet.getRow(0);
        // 得到列
        Cell cell = row.getCell(1);

        // 读取值的时候，一定需要注意类型
        // getStringCellValue 字符串类型

        System.out.println(cell.getNumericCellValue());

        // 关闭流
        inputStream.close();


    }

}
~~~



















