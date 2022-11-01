Dockerfile 技巧——合理使用缓存
===============================

Dockerfile 在执行时会帮你做一些 cache，下次执行 build 时会加速，但更新 Dockerfile 某一行后，下次 build 时后面的程式码都不会执行 cache。

解决办法是可以把比较常改变的程式码往下移动: 例如范例中将 COPY 往下移动

.. code-block:: Dockerfile

  FROM python:3.9.5-slim

  RUN pip install flask // 安装 flask

  WORKDIR /src // 切换当前档案路径到 /src
  ENV FLASK_APP=app.py
  
  COPY app.py /src/app.py // 複製本地的 app.py 到 image 的 /src/app.py

  EXPOSE 5000 // 对外暴露一个 port

  CMD ["flask", "run", "-h", "0.0.0.0"]
  // 0.0.0.0，搭配前面 EXPOSE 的 port 以及截图的指令，就可以在主机访问网站

