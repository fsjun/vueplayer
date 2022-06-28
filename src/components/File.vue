<template>
    <div class="hello">
        <h1>{{ msg }}</h1>
        <p>
            <input type="file" v-on:change="fileSelect3" multiple="multiple" id="files" />
        </p>
        <div id="list1"></div>
        <div id="preview2"></div>
        <div id="img-box"></div>
    </div>
</template>

<script>
export default {
    name: 'HelloWorld',
    props: {
        msg: String
    },
    methods: {
        fileSelect1: function (e) {
            var files = e.srcElement.files;
            var html = []
            for (var i = 0, len = files.length; i < len; i++) {
                var f = files[i];
                console.log(f);
                html.push(
                    '<p>',
                    f.name + '(' + (f.type || "n/a") + ')' + ' - ' + f.size + 'bytes',
                    '</p>'
                );
            }
            document.getElementById('list1').innerHTML = html.join('');
        },
        fileSelect2: function (e) {
            var files = e.srcElement.files;
            var p = document.getElementById('preview2');

            for (var i = 0, f; (f = files[i]); i++) {
                var reader = new FileReader();
                reader.onload = (function (file) {
                    return function (e) {
                        console.log(e);
                        console.log(file);
                        console.log(this);
                        var span = document.createElement('span');
                        span.innerHTML = '<img src="' + this.result + '" alt="' + file.name + '" />';

                        p.insertBefore(span, null);
                    };
                })(f);
                //读取文件内容
                reader.readAsDataURL(f);
            }
        },
        fileSelect3: function (e) {
            var files = e.srcElement.files;
            var imgBox = document.getElementById('img-box');

            for (var i = 0, len = files.length; i < len; i++) {
                var f = files[i];
                var objectURL = URL.createObjectURL(f);
                console.log(objectURL);
                var img = new Image();
                img.onload = () => {
                    URL.revokeObjectURL(objectURL);
                }
                img.src = objectURL;
                imgBox.appendChild(img);
            }
        },
        isSupportFileApi: function () {
                if (window.File && window.FileList && window.FileReader && window.Blob) {
                    return true;
                }
                return false;
            }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
