# MeiZuMusciApi
魅族音乐Api


`分页起始位置` 和 `分页数量` 都是`int ` 值，版本号我这里是 `5.0 `, 所有未标注的请求方式都死`GET` 请求

`API_HOST` :  `http://fast.y.meizu.com/`

- 启动根据版本号拿到版本id

  `/open/api/v2/ui/index.do?v={版本号}`

- 根据版本id返回的首页id去请求乐库id

  `/open/api/v2/ui/detail.do?id={版本id}&start=0&limit=1`

- 根据返回的乐库id请求乐库数据

  `/open/api/v2/ui/detail.do?id={乐库id}&start=0&limit=10`


- 乐库首页

  - 音乐播放（播放需要参数，参数是反编译，读源码出来的,有些不准确）

    `POST` 请求`/open/api/v2/song/getAudioInfo/{musicId}/0/1/19220771`

    `params`

    `stamp` : deviceId + `EBL22MDZK`(没读出来这段) + `System.currentTimeMillis()`

    `version ` : `v2`

    `_musiccodeurl` : `/open/api/v2/song/getAudioInfo/{musicId}/0/1/19220771`

    `sign` ：`MD5加密字符串`

    sign 需要做点处理，将上面三个的`key` 按照`ASCII` 拍下序，然后

    ```java
      StringBuilder localStringBuilder = new StringBuilder("MEIZU");
            for (int i = 0; i < paramList.size(); i++) {
                String value = paramList.get(i).getValue(); //将每个value拼接上去
                localStringBuilder.append(value);
                if (i < paramList.size() - 1) {
                    localStringBuilder.append("@"); //然后以@拼接
                }
            }
      localStringBuilder.append("MUSIC"); //最后拼接上MUSIC
      MD5Util.parseStrToMd5L32(localStringBuilder.toString()); //最后将字符串转换成32为的MD5小写字符串
    ```

    ​


- 搜索

  `/open/api/v2/search/vsearch.do?q={搜索key}&geoloc=null&include_optimum=true&page={分页数目}&limit={分页数量}&installed_apps=[]&search_Types=[3]`


- 歌单（`start` 是分页标志,双数[0,2,4,6]）

  - 热门歌单: 

    `/open/api/v2/ui/content.do?id={热门歌单id}&start={分页起始位置}&limit={分页数量}`

  - 歌单活动：

    `/open/api/v2/ui/detail.do?id={歌单活动id}&start={分页起始位置}&limit={分页数量}`

    - 歌单详情

      - 歌单详情: 

        `/open/api/v2/songList/detail/{歌单id}}`

      - 歌单详情列表：

        `/open/api/v2/songList/songs/{歌单id}}/{分页起始位置}}/{分页数量}`

      - 歌单评论列表：

        `/open/api/v2/comment/commentList.do?id={歌单id}}&type={type}&source=0&startId=0&limit={分页数量}`


- 排行榜：

  `open/api/v2/ui/detail.do?id={排行榜id}&start={分页起始位置}&limit={分页数量}`

  - {类别}榜

    `/open/api/v2/category/getCategoryRes.do?categoryId={类别id}&start={分页起始位置}&limit={分页数量}&startId=0`

- 歌曲电台(电台播放暂时还未去抓取)

  - 获取电台类别id

    `/open/api/v2/ui/detail.do?id={电台id}&start={分页起始位置}&limit={分页数量}`

  - {电台类别id}

    `/open/api/v2/ui/detail.do?id={类别id}&start={分页起始位置}&limit={分页数量}`

  - 电台详情

    `/open/api/v2/category/getCategoryRes.do?categoryId={电台id}&start={分页起始位置}&limit={分页数量}&startId=0`
