---
​---
title: 计算机网络
date: 2024-08-09 13:50:00 +0800
description: java start process
categories: [java]
tags: [process]
​---
---

# java 启动进程

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.util.concurrent.TimeUnit;

public class ProcessBuilderExample {
    public static void main(String[] args) {
        try {
            ProcessBuilder processBuilder = new ProcessBuilder("sh", "test.sh");
            processBuilder.redirectErrorStream(true);
            Process process = processBuilder.start();
            Thread thread = new Thread(() -> {
                try (InputStream inputStream = process.getInputStream();
                     BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream))) {
                    String line;
                    while ((line = reader.readLine()) != null) {
                        System.out.println(line);
                    }
                } catch (IOException e) {
                    System.out.println("read error");
                }
            });

            thread.start();
            long timeout = 5;
            TimeUnit timeUnit = TimeUnit.SECONDS;
            if (!process.waitFor(timeout, timeUnit)) {
                process.destroy();
                System.out.println("Command timed out and was terminated.");
            }

            int exitValue = process.exitValue();
            System.out.println("Exit value: " + exitValue);

        } catch (IOException | InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```



