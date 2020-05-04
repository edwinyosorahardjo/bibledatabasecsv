# BZip2 Decoder
*.bz2 files are compressed using [BrotliSharpLib](https://github.com/master131/BrotliSharpLib)

 Decompression can be done using the same library  
 or Javascript Library [bzip2-js](https://github.com/kirilloid/bzip2-js)

 ```
     <!--BZ2 decode-->
    <script src="bz2.js"></script>
    <script>

        var makeRequest = function (uri, method) {
            var request = new XMLHttpRequest();
            // Return it as a Promise
            return new Promise(function (resolve, reject) {
                request.onload = function () {

                    // Process the response
                    if (request.status >= 200 && request.status < 300) {
                        // If successful
                        console.log('request-received');
                        resolve(request);
                        //resolve(request);
                    } else {
                        // If failed
                        reject({
                            status: request.status,
                            statusText: request.statusText
                        });
                    }
                }

                request.open('GET', uri, true);
                request.responseType = "arraybuffer";
                request.send();
            });
        }

        function getBz2Json(url) {
            makeRequest(url)
                .then(function (post) {

                    console.log('Success!', post);
                    var data = post.response;
                    var databytes = window.bz2.decompress(new Uint8Array(data));
                    var dataresult = new TextDecoder('utf8').decode(databytes);
                    var jsonObj = JSON.parse(dataresult);
                    console.log(jsonObj);

                    delete data;
                    delete databytes;
                    delete dataresult;
                    delete jsonObj;
                })
                .catch(function (error) {
                    console.log('Something went wrong', error);
                });
        }

        getBz2Json('../../db/b_kjv.bz2');
    </script>

 ```
