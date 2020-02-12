# javaMethods
>## 1.读取一个文件的后n行数据,返回每一行`String`的集合
```
public static List<String> readLastNLine(File file, long numRead) {
        List<String> result = new ArrayList<String>();
        long count = 0;//定义行数
        if (!file.exists() || file.isDirectory() || !file.canRead()) {
            return null;
        }
        RandomAccessFile fileRead = null;
        try {
            fileRead = new RandomAccessFile(file, "r"); //用读模式
            long length = fileRead.length();//获得文件长度
            if (length == 0L) {//文件内容为空
                return result;
            } else {
                // 开始位置
                long pos = length - 1;
                while (pos > 0) {
                    pos--;
                    fileRead.seek(pos); // 开始读取
                    if (fileRead.readByte() == '\n') {//有换行符，则读取
                        String line = new String(fileRead.readLine().getBytes("ISO-8859-1"), "UTF-8");
                        result.add(line);
                        count++;
                        if (count == numRead) {//满足指定行数 退出。
                            break;
                        }
                    }
                }

                if (pos == 0) {
                    fileRead.seek(0);
                    result.add(new String(fileRead.readLine().getBytes("ISO-8859-1"), "UTF-8"));
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (fileRead != null) {
                try {
                    // 关闭资源
                    fileRead.close();
                } catch (Exception e) {
                }
            }
        }

        return result;
    }
```
