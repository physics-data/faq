## 向GitHub Classroom Workflow中添加 dependencies

使用 nano 等编辑器，修改`.github/workflows/classroom.yml`文件。在`steps:`下添加代码，注意合理缩进，结果为：

```yml
...
steps:
  - name: Install dependencies
    run: |
      pip3 install numpy
...
```

其中`numpy`可以替换为任意可用`pip`安装的package。

其他更复杂的配置，可见 GitHub Docs 中的 [GitHub Actions Documentation](https://docs.github.com/en/actions) 页面。
