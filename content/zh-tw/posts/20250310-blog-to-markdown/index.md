---
title: "部落格搬家&轉檔技術分享"
date: 2025-03-10
description: "從wordpress.com轉到hugo用md寫"
---

## 緣起
雅青一直推薦我hugo，於是我也來試試看

那我原本網誌是 wordpress.com上面的格式，就開始思考怎麼轉檔

因為用markdown在vscode裡面寫我覺得是一個滿好的寫作體驗，比起用wordpress的editor
## 概念流程

匯出文章 -> 轉 markdown -> 發布hugo

由於之前已經串好github pages, 只要發文就可以

那這次首先從xml (附錄)裡面找到 wp:post_date, title, content:encoded這幾項

然後轉成markdown, 存成md那就開始採坑吧

首先是檔名處理
專案架構是 {folder-name}/index.md

所以要先把中文的title轉成可用的資料夾名稱

直接用python 打openai api

```python
def sanitize_filename(name):
    name = name.replace("…", "").replace(".", "")
    # Replace invalid characters with underscores
    name = re.sub(r'[<>:"/\\|?*]', "_", name)
    msg = f"Give me a brief name of this title: {name}, use only three english word, separate with -,no space, the name only, such as my-first-post"
    result = send_user_message(msg)
    name = result["data"]["content"]
    print(name)
    return name
```
後來為了好管理，我又加了一步驟，把檔名改成年月日開頭 "20230824NET-Integration-nUnit"
**坑**

1. yaml
例如這篇的設定
```yaml
---
title: "部落格搬家&轉檔技術分享"
date: 2025-03-10
description: "從wordpress.com轉到hugo用md寫"
---
```

有特殊字元需要前後加上 " 變成字串
用python一樣抓 第二行 第四行硬改就可以

```python
def update_markdown_file(file_path):
    with open(file_path, "r", encoding="utf-8") as file:
        lines = file.readlines()

    if len(lines) >= 4:  # Ensure the file has at least 4 lines
        # Process the second line (title)
        title_value = lines[1].replace("title:", "").strip()
        lines[1] = f'title: "{title_value}"\n'

        # Process the fourth line (description)
        description_value = lines[3].replace("description:", "").strip()
        lines[3] = f'description: "{description_value}"\n'

        # Write the modified content back to the file
        with open(file_path, "w", encoding="utf-8") as file:
            file.writelines(lines)

```

2. image
圖片在用py套件 html2text 的時候被截斷了
用GPT再小改一下 20分鐘完成
因為檔名已經用GPT改過 所以不想浪費token 從xml重新再來跑
就直接抓檔案來跑

```python
# Function to fix broken image links
def fix_broken_image_links(file_path):
    with open(file_path, "r", encoding="utf-8") as file:
        lines = file.readlines()

    fixed_lines = []
    buffer = ""  # Temporary buffer to store incomplete image lines

    for line in lines:
        stripped_line = line.strip()

        if buffer:
            # If buffer is not empty, try to continue the previous broken line
            print(stripped_line)
            buffer += stripped_line  # Append the current line to buffer

            if stripped_line.endswith(")"):
                # If the line now forms a complete image link, store it and clear the buffer
                if buffer:
                    fixed_lines.append(buffer)
                buffer = ""
            continue

        if stripped_line.startswith("![](") and not stripped_line.endswith(")"):
            print("start:", stripped_line)
            # Detected the start of a broken image link
            buffer = stripped_line
        else:
            if line:
                fixed_lines.append(line)  # Normal line, just add it

    # If there's still a buffer left (in case of edge cases), add it back
    if buffer:
        fixed_lines.append(buffer)
    # print(fixed_lines)
    # Write the fixed content back to the file
    with open(file_path, "w", encoding="utf-8") as file:
        file.write("\n".join(fixed_lines) + "\n")
```
## 結語

有了GPT的幫助，很快就可以完成以前覺得很久的工作

從讀取xml 到轉檔、處理文字

不過在改名的時候一開始GPT用regular expression處理一直沒抓到，我才改用暴力 第二行第四行的硬處理比較快

給他指令就產出來了

如果加上copilot 或是 cursor編輯器應該可以更快
## 附錄：匯出的檔案內容

```xml
<item>
  <title><![CDATA[Adam-Opel Strasse- The room of the King and Prince]]></title>
  <link>http://jaythecheyi.home.blog/2017/04/28/adam-opel-strasse-the-room-of-the-king-and-prince/</link>
  <pubDate>Fri, 28 Apr 2017 21:08:00 +0000</pubDate>
  <dc:creator>soarwing52hot</dc:creator>
  <guid isPermaLink="false">http://jaythecheyi.home.blog/2017/04/28/adam-opel-strasse-the-room-of-the-king-and-prince/</guid>
  <description/>
  <content:encoded><![CDATA[It all starts at the day I moved in, 2016/10/01<br /><br />a new, adventurous life awaits in this building<br /><br />and the story is still going<br /><br />this, is the house I'm going to live through my master.<br /><br /><div class="separator" style="clear:both;text-align:center;"><a href="https://jaythecheyi.home.blog/wp-content/uploads/2019/11/ab4b7-img_0999.jpg" style="margin-left:1em;margin-right:1em;"><img border="0" height="320" id="id_aeda_3fea_dc09_eda6" src="https://jaythecheyi.home.blog/wp-content/uploads/2019/11/40a07-img_0999.jpg" style="height:auto;width:240px;" width="240" /></a></div><br /><a name='more'></a><br /><br /><a href="https://draft.blogger.com/null" name="more"></a>Well, that is the European center bank, this is my real house.<br /><br /><div class="separator" style="clear:both;text-align:center;"><a href="https://jaythecheyi.home.blog/wp-content/uploads/2019/11/24f99-img_2049.jpg" style="margin-left:1em;margin-right:1em;"><img border="0" height="240" id="id_5761_abbe_d30d_382a" src="https://jaythecheyi.home.blog/wp-content/uploads/2019/11/5b1f0-img_2049.jpg" style="height:auto;width:320px;" width="320" /></a></div><br />This story is really hard to start, because this is just too much to say,<br /><br />so just like the old story of all times.<br /><br />Once upon a time, there was one house called Adam-Opel 24<br /><br />It was built in the 1960s, used for old bachelors.<br /><br />There were so many cool guys living there,<br /><br />like Alex, who is always drunk and knocking on our door.<br /><br />and others heard from Dustin,<br /><br />like there was one guy called the phoenix, tried to jump and kill himself from first floor<br /><br />once a black guy on drug and banged the kitchen.<br /><br />some fellows go in and out the jail frequently.<br /><br />A lot of old men R.I.P. in their room.<br /><br />so, the life here, I'm looking forward to it!<br />--------------------------------------------------------------------------------------------------------------------------<br />Now to introduce my friends in this block.<br /><div>This is a block of eight rooms, with two double room. So that makes us ten in this block. </div><div><br /></div><div>401 is rented by one Vietnamese guy who never here. </div><div><br />402 is my room. </div><div><br />403 is Kamal, engineer student in our school. He's here already three years. A funny guy. </div><div><br />404 is two Russian guys working here. They don't speak English nor German. So it's hard to communicate. They often stand outside and smoke. I don't really like that. </div><div><br />405 is two Indian tall guys. They used to held one India kitchen almost every night. Usually makes a mess. Which kind of irritated others. </div><div><br />406 is Dustin. The American German, most of the parties and events were held by him. His nickname is the prince, lol. When there is a prince, there will be a king. </div><div>Where's the king? His moving out is why I get this room. Is a sad story but I'll tell it later. </div><div><br />407 is Weby. Another Indian. Got this name cause his arms and legs are long and thin. </div><div><br />408 is Enzel. He's 61 years old. His biggest hobby is to open his door and watch TV with the sound loud. </div><div>But what does he watch? Porn. He has two screens, always one porn. Other can be football games or somewhat. </div><div><br />These are the fellows living in this block. We share toilet, shower and kitchen. </div><div>--------------------------------------------------------------------------------------------------------------------------<br /><br />Okay, I think I'll start with the story of the king. After all, he's the one I can live into this place.<br /><br />He's the man who lives here for the longest time, 35 years.<br /><br />He came from Italy, a place "with 200 km coastline".He was a worker at the company nearby.<br /><br /></div><div>According to Dustin, he already got Alzheimer's disease when he moved in. </div><div><br /></div><div>So he doesn't really remember what happened before that. </div><div>But he know everyone one in the building. And since everyone knows him, and show him respect, he is the king of this building.<br /><br />When you really hug him you can feel the warm sun in his blood, he may not be your grandpa, but he is  definitely an old man everyone like.<br /><br />But actually it's really hard to describe, it's a atmosphere, which you can only feel it.<br /><br /></div><div>That's how this name came.<br /><div style="text-align:center;"><span style="font-size:large;">The KING </span></div></div><div>--------------------------------------------------------------------------------------------------------------------------</div><div>And who's next? Dustin. </div><div><br /></div><div>He lived here for seven years. And enjoyed. </div><div><br /></div><div>He's usually the party planner and holder. </div><div><br /></div><div>He's American-German so we usually use English to communicate. </div><div><br /></div><div>He is a good friend to the king, and he says he the prince of this block.<br />--------------------------------------------------------------------------------------------------------------------------<br />We had so many parties<br /><div class="separator" style="clear:both;text-align:center;"><a href="https://jaythecheyi.home.blog/wp-content/uploads/2019/11/19b56-img_0005.jpg" style="margin-left:1em;margin-right:1em;"><img border="0" height="240" src="https://jaythecheyi.home.blog/wp-content/uploads/2019/11/2cf49-img_0005.jpg" width="320" /></a></div> Birthday<br /><div class="separator" style="clear:both;text-align:center;"><a href="https://jaythecheyi.home.blog/wp-content/uploads/2019/11/0d479-img_1800.jpg" style="margin-left:1em;margin-right:1em;"><img border="0" height="320" src="https://jaythecheyi.home.blog/wp-content/uploads/2019/11/7e23c-img_1800.jpg" width="180" /></a></div><div class="separator" style="clear:both;text-align:center;"><br /></div><div class="separator" style="clear:both;text-align:center;">Thanksgiving-Chrismas</div><div class="separator" style="clear:both;text-align:center;"><br /></div><div class="separator" style="clear:both;text-align:center;"><a href="https://jaythecheyi.home.blog/wp-content/uploads/2019/11/15991-img_2092.jpg" style="margin-left:1em;margin-right:1em;"><img border="0" height="320" src="https://jaythecheyi.home.blog/wp-content/uploads/2019/11/7afcb-img_2092.jpg" width="240" /></a></div><br /><div class="separator" style="clear:both;text-align:center;"><a href="https://jaythecheyi.home.blog/wp-content/uploads/2019/11/73786-img_2098.jpg" style="margin-left:1em;margin-right:1em;"><img border="0" height="180" src="https://jaythecheyi.home.blog/wp-content/uploads/2019/11/6f8a5-img_2098.jpg" width="320" /></a></div><div class="separator" style="clear:both;text-align:center;"><br /></div><div class="separator" style="clear:both;text-align:center;">It's all really cool and enjoy of life</div></div><div>--------------------------------------------------------------------------------------------------------------------------</div><div>And then, Kamal. </div><div><br /></div><div>He came from India and study software engineering. </div><div><br /></div><div>Usually he's home all the time. </div><div><br /></div><div>At first he worked at a kiosk every weekend so he don't really have free time. </div><div><br /></div><div>But he shows great support as a friend to our block. </div><div><br /></div><div>He just turned 29 this year, we had a small celebration with one cool video. </div><div><div class="separator" style="clear:both;text-align:center;">[youtube=https://www.youtube.com/watch?v=w7b4lVuyVhk&amp;w=320&amp;h=266]</div><br /></div><div>--------------------------------------------------------------------------------------------------------------------------</div><div>Living in this house is really interesting. </div><div><br /></div><div>Comparing all the student dorm and the house provided by the school, this house is really cheap and nice. </div><div>Most my classmates are in the FDH, they don't have elevator and the room is smaller. </div><div>We got one built in 1963, still functions. </div><div><br /></div><div>Though most people here are Indian students, to meet other friend I need to go for at least 30 minutes to the city, but it's okay. </div><div><br /></div><div>The traffic here is only two tram, 11 and 12. </div><div>But they work perfectly, goes to school, Hauptbahnhof, Zeil without transferring. </div><div>--------------------------------------------------------------------------------------------------------------------------</div><div>Sometimes we hangout in the house with some drinks and wraps. </div><div><br /></div><div>We got Adam, Marie also live at this house. </div><div>But most of the people Dustin knows moved out. </div><div>It understandable cause it's only one room, most people will prefer live a better place when they got the money. </div><div>Two Chris, Depak, Salva are the people we hangout together. </div><div>--------------------------------------------------------------------------------------------------------------------------</div><div>That's the house, Adam-Opel. </div><div><br /></div><div>The place I start my life in Germany. </div><div><br /></div><div>I would say I'm lucky to be in here, though to keep sanity is really not an easy thing in this insane house. </div><div><br /></div><div>But I'm going to stay longer in this house, and cause I can keep it after graduating from school. </div><div><br /></div><div>And the fun of it, you need to create it. And it will keep creating more life!</div>]]></content:encoded>
  <excerpt:encoded><![CDATA[]]></excerpt:encoded>
  <wp:post_id>1885</wp:post_id>
  <wp:post_date>2017-04-28 23:08:00</wp:post_date>
  <wp:post_date_gmt>2017-04-28 21:08:00</wp:post_date_gmt>
  <wp:post_modified>2017-04-28 23:08:00</wp:post_modified>
  <wp:post_modified_gmt>2017-04-28 21:08:00</wp:post_modified_gmt>
  <wp:comment_status>open</wp:comment_status>
  <wp:ping_status>open</wp:ping_status>
  <wp:post_name>adam-opel-strasse-the-room-of-the-king-and-prince</wp:post_name>
  <wp:status>publish</wp:status>
  <wp:post_parent>0</wp:post_parent>
  <wp:menu_order>0</wp:menu_order>
  <wp:post_type>post</wp:post_type>
  <wp:post_password/>
  <wp:is_sticky>0</wp:is_sticky>
  <category domain="post_tag" nicename="english-posts"><![CDATA[English posts]]></category>
  <category domain="post_tag" nicename="%e9%9b%99%e8%aa%9e%e7%b4%80%e9%8c%84-%e6%b3%95%e8%98%ad%e7%94%9f%e6%b6%af"><![CDATA[雙語紀錄-法蘭生涯]]></category>
  <category domain="post_tag" nicename="%e9%9d%88%e9%ad%82%e7%9a%84%e5%81%b4%e5%af%ab"><![CDATA[靈魂的側寫]]></category>
  <category domain="post_tag" nicename="gis"><![CDATA[GIS]]></category>
  <category domain="category" nicename="uncategorized"><![CDATA[Uncategorized]]></category>
  <wp:postmeta>
    <wp:meta_key>_publicize_pending</wp:meta_key>
    <wp:meta_value><![CDATA[1]]></wp:meta_value>
  </wp:postmeta>
  <wp:postmeta>
    <wp:meta_key>_import_session_id</wp:meta_key>
    <wp:meta_value><![CDATA[5dc935284e719]]></wp:meta_value>
  </wp:postmeta>
  <wp:postmeta>
    <wp:meta_key>_import_original_post_id</wp:meta_key>
    <wp:meta_value><![CDATA[0]]></wp:meta_value>
  </wp:postmeta>
</item>
```