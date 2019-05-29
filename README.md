# firstProject

node执行命令中改端口： set port=8001 && npm start

#20190529

downloadPackFile (node) { // js下载
    if (!this.act.pack[node.data.id]) {
      return
    }
    if (this.act.pack[node.data.id].status !== 'SUCCESS') {
      return
    }
    const taskId = this.act.pack[node.data.id].taskId
    this.$store.dispatch('gdocUploader/getDownloadUrl', { taskId }).then(res => {
      console.log('gdocUploader/getDownloadUrl:', res)
      if (res.data.code === 'SUCCESS') {
        // 下载zip
        const downloadElement = document.createElement('a')
        downloadElement.href = res.data.detail
        downloadElement.download = `${this.act.pack[node.data.id].fileName}` // 下载后文件名
        document.body.appendChild(downloadElement)
        downloadElement.click() // 点击下载
        document.body.removeChild(downloadElement) // 下载完成移除元素
      }
    })
  },
  downloadDrawingZip (data) { // js下载文件流
    // window.open('/api/file/gDocFile/batchDownLoadZipFile?docId=' + data.id, '', '_blank')
    this.$store.dispatch('getBatchDownLoadZipFile', data.id).then(res => {
      // js 保存文件流
      const blob = new Blob([res.data])
      const downloadElement = document.createElement('a')
      const href = window.URL.createObjectURL(blob) // 创建下载的链接
      downloadElement.href = href
      downloadElement.download = `${data.name}.zip` // 下载后文件名
      document.body.appendChild(downloadElement)
      downloadElement.click() // 点击下载
      document.body.removeChild(downloadElement) // 下载完成移除元素
      window.URL.revokeObjectURL(href) // 释放掉blob对象
    }).catch(err => {
      console.error('下载压缩包失败: ', err)
    })
  }

