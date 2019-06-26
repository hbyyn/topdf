<template>
  <div>
    <button @click="allPdf">allPdf</button>
    <button @click="nowPdf">nowPdf</button>
    <div
      class="home"
      id="faceDataTable"
    >
      <h1>adhalsdfhlakhsflkahlafhsklafksssssss</h1>
    </div>


  </div>
</template>

<script>
import html2canvas from "html2canvas"
import jsPDF from "jspdf"
export default {
  methods: {
    allPdf() {
      html2canvas(document.getElementById("faceDataTable")).then(function (canvas) {
        var contentWidth = canvas.width;
        var contentHeight = canvas.height;

        //一页pdf显示html页面生成的canvas高度;
        var pageHeight = contentWidth / 592.28 * 841.89;
        //未生成pdf的html页面高度
        var leftHeight = contentHeight;
        //页面偏移
        var position = 0;
        //a4纸的尺寸[595.28,841.89]，html页面生成的canvas在pdf中图片的宽高
        var imgWidth = 555.28;
        var imgHeight = 552.28 / contentWidth * contentHeight;

        var pageData = canvas.toDataURL('image/jpeg', 1.0);

        var pdf = new jsPDF('', 'pt', 'a4');

        //有两个高度需要区分，一个是html页面的实际高度，和生成pdf的页面高度(841.89)
        //当内容未超过pdf一页显示的范围，无需分页
        if (leftHeight < pageHeight) {
          pdf.addImage(pageData, 'JPEG', 20, 0, imgWidth, imgHeight);
        } else {
          while (leftHeight > 0) {
            pdf.addImage(pageData, 'JPEG', 20, position, imgWidth, imgHeight)
            leftHeight -= pageHeight;
            position -= 841.89;
            //避免添加空白页
            if (leftHeight > 0) {
              pdf.addPage();
            }
          }
        }

        pdf.save('全部.pdf')

      })
    },
    nowPdf() {
      html2canvas(document.getElementById("faceDataTable")).then(function (canvas) {

        //返回图片dataURL，参数：图片格式和清晰度(0-1)
        var pageData = canvas.toDataURL('image/jpeg', 1.0);

        //方向默认竖直，尺寸ponits，格式a4[595.28,841.89]
        var pdf = new jsPDF('', 'pt', 'a4');

        //addImage后两个参数控制添加图片的尺寸，此处将页面高度按照a4纸宽高比列进行压缩
        pdf.addImage(pageData, 'JPEG', 20, 0, 555.28, 552.28 / canvas.width * canvas.height);

        pdf.save('单页.pdf');

      })
    }
  }
}

</script>
<style type="text/css" lang="scss">

.home {
  width: 1000px;
  ul {
    height: 20px;
    display: flex;
    margin: 0 100px;
    li {
      flex: 1;
      height: 20px;
      line-height: 20px;
      list-style: none;
      border-bottom: 1px solid black;
      border-right: 1px solid black;
    }
    li:nth-child(1) {
      border-left: 1px solid black;
    }
  }
  p {
    text-align: center;
  }
}
</style>