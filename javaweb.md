## servlet的生命周期
1. 只有一个Servlet对象（要点）
2. 第一次请求的时候被初始化，只此一遍
3. 初始化后先调用init方法，只此一遍
4. 每个请求，调用一遍service -> service -> doGet/doPost。以多线程的方式运行
5. 卸载前调用destroy方法

## jsp技术

## servlet规范

## cookie和session

## ajax

## filter

## listener

## interceptor

## jdbc

## xml