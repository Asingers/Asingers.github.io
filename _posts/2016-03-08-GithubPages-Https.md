---
layout: post
title: "GithubPages HTTPS"
subtitle: "解读"
date: 2016-03-08
author: "Asingers"
header-img: "http://7xqmgj.com1.z0.glb.clouddn.com/header-imghttps.jpg"
categories: life
tags:
    - Life
    - Github
---
**你大概知道什么意思就行,当然你也可以看英文版**

<select onchange="onLanChange(this.options[this.options.selectedIndex].value)">
    <option value="0" selected="selected"> 中文 Chinese </option>
    <option value="1"> 英语 English </option>
</select>

<!-- Chinese Version -->
<div style="display: block;" class="zh">

   最近，GitHub页面开始支持HTTPS为github.io *。域，如这一个和这一个。在我看来，这是一个巨大的交易- GitHub页面是唯一主要的，免费的，通用的Web主机也提供了一个安全通道。鉴于有很多简单而强大的应用GitHub页面，如何重要的安全和保密的连接到网络的未来，我认为这是一大进步，开发商，GitHub上，和日常生活的人. <hr>
有两个主要的渔获：
你不能强迫HTTPS为你的github.io *。站点
你不能使用一个自定义的域名与一个完全安全的HTTPS连接。
但你可以使用一个https://yourname.github.ioURL。所以让我们一起工作，我们得到了什么！
<hr>
默认HTTPS
如果你有一个GitHub页面的网站，你想它运行接近HTTPS只可能：

始终链接到自己的网站使用它http：/ /URL。通过任何支持的网站或博客文章或材料和更新链接。
如果有任何外部和突出的链接到你的网站，让网站所有者更新链接使用http：/ /。
如果你是一个能干的人，做一个GitHub的搜索有人链接到你的域名文件浏览器，和一些快速切换的链接系统。
<hr>
告诉搜索引擎
使用典型的URL告诉搜索引擎你的网站使用HTTPS版。

<链接 REL =“规范” href =“https://yoursite.github.io” />
在基尔，这看起来像：

<链接 REL =“规范” href =“{ { { { page.url site.url } } } }” />
你在哪里设置URL在你的网站的领域_config.yml文件看到在 这里和在 这里为例。

（为尼龙谢谢这！）
<hr>
强迫一个重定向
那么，你如何强制重定向一个静态的网站时，你不能控制服务器？你不能使用< Meta刷新>标签，因为你不能改变什么HTML获取每协议。
<hr>
唯一的选择是在JavaScript中做。下面放在HTML上，更换若与您的用户或组织名称：
VaR 主机 = 若IO。GitHub上。”；
如果 （（主机 = = 窗口。位置。主机） & （窗口。位置。协议 ！= “http”））
    窗口。位置。协议 = “https”；
上面提到的方法将立即重定向到任何用户访问你的域名的安全协议安全协议。它不会影响游客对其他领域（如本地主机）。这就是我们正在做的阳光基金会现在我们的文件阳光国会API。

不犯错误，客户端重定向是一个黑客和任何一个不完善的解决方案。这也是不安全的！但最重要的是要确保链接到你的网站会让其他人前进是最安全的一种。链接越多，你和别人做，直接去http：/ /，较少的重定向需要触发。
如果你使用的化身，你可能希望按照我的例子使用site.enforce_ssl参数，或者你可以来像上面。要是这能烤成杰基尔插件是更好的，但GitHub页面只支持一对夫妇的内插件。

使用自定义域与CloudFlare
[更新：重写这段后来在2014后发布免费的SSL，CloudFlare的计划。]

CloudFlare现在提供的免费的SSL终止任何网站，他们的主人，他们的“通用的SSL”计划下。
<hr>
有一点要注意：

免费计划只使用现代技术标准（ECDSA，SHA-2，SNI），所以访问使用Windows XP和/或旧的Internet Explorer浏览器的客户可能无法连接。
直到GitHub页面或首先解决的事情，你只能加密用户和CloudFlare的关系。CloudFlare和GitHub页面之间的连接将需要保持在明文，使用CloudFlare的“灵活的SSL”选项。更多的信息，为什么是这样的话，读这些 评论那些展示了它是如何工作的。
如果你不支持古老的客户，并与CloudFlare和GitHub之间明确的文字流动的流量，你能在你的GitHub页面网站的自定义域前免费SSL。

但小心。这是一个诱人的，但不完整的解决方案。我是很失望看到五月天superpac使用此配置，募捐。攻击者可能会首先和GitHub之间可以改变的形式发送付款信息到任何地方的交通要。

更普遍的问题是，浏览器无法显示给用户的连接不完全安全。现在，人们普遍认为，如果在浏览器中有一个锁的标志，连接是安全的和网站之间的。CloudFlare的灵活的SSL配置打破了这种期望，也没有办法从外面来判断一个网站使用它。

我得说，我同意“__agwa- CloudFlare的初衷是好的，但其灵活的SSL冲淡了HTTPS的价值：https://t.co/6b3dqmzhgk

-埃里克轧机（@ konklone）2014年12月1日
我不是说不要使用CloudFlare的灵活的SSL -但知道你在做什么，和沟通，首先他们需要添加为外人知道的连接是如何安全的方式。
<hr>
GitHub可以做什么
这是不可能的，Github去创造必要的基础设施来支持自定义域，和CloudFlare的领导将使少的问题。

然而，仍然有一些东西可以降低门槛，GitHub使用HTTPS和使人们更加意识到。

文档的特征。没有正式的公告或说明SSL的GitHub页面；它可能消失的任何时间。这是值得一个帖子和帮助页面，水泥GitHub的承诺和如何避免常见的混合内容的陷阱。
允许自定义域的SSL配置的地方。如上所述，即使你配置你的自定义域服务像CloudFlare的和点一个CNAME在GitHub页面，你会得到一个错误，一个需要行动在GitHub上的部分固定。
支持SSL配置。 github.com的SSL世界级的，但github.io的还可以使用一些调整在向前保密和密码的选择。
最重要的是，让用户打开HTTPS默认情况下，用一个复选框的设置，部队将在服务器级别。这将使整个破解上述不必要的，并导致更多的人去做吧。事实上，打开复选框的新用户和现有用户的默认，谁也没有任何GitHub页面！
2014可能是SSL的一年，和GitHub是肥沃的土壤，扩大运动场。如果你想推进，电子邮件support@github.com在GitHub页面索要正规的HTTPS支持。（引用这篇博客不能伤害。）

在这期间，我一直在文件系统和开幕式门票在GitHub页面网站我在乎，南瓜混合内容的警告和确保他们的HTTPS准备。去采取一些网站你知道GitHub页面做同样的！


</div>

<!-- English Version -->
<div style="display: none;" class="en">

    <p>Recently, GitHub Pages<a href="https://twitter.com/benbalter/status/444555263195217920">began supporting HTTPS</a>for<code>*.github.io</code>domains, like<a href="https://cfpb.github.io/">this one</a>and<a href="https://sunlightlabs.github.io/congress/">this one</a>.</p>

<p>In my opinion, this is a huge deal - GitHub Pages is the only major, free, general-purpose web host which also offers a secure channel. Given<a href="https://konklone.com/post/the-power-and-potential-of-github-pages">how many simple and powerful uses</a>GitHub Pages has, and how vital secure and confidential connections are to the future of the web, I see this as a big step forward for developers, GitHub, and everyday people.</p>

<p>There are two major catches:</p>

<ul>
<li>You<strong>can&#39;t force HTTPS</strong>for your<code>*.github.io</code>site.</li>
<li>You<strong>can&#39;t use a custom domain name</strong>with a fully secured HTTPS connection.</li>
</ul>


<p>But you<strong>can</strong>use an<code>https://yourname.github.io</code>URL. So let&#39;s work with what we&#39;ve got!</p>

<h3>Defaulting to HTTPS</h3>

<p>If you have a GitHub Pages site, and you&#39;d like it to run as close to HTTPS-only as possible:</p>

<ul>
<li>Always link to your own site using its<code>https://</code>URL. Go through any supporting sites or blog posts or material and update the link.</li>
<li>If there are any external and prominent links to your site, ask the site owner to update their link to use<code>https://</code>.</li>
<li>If you&#39;re a go-getter, do a GitHub search for<a href="https://github.com/search?q=%22sunlightlabs.github.io%22&amp;amp;ref=cmdform&amp;amp;type=Code">anyone linking to your domain</a>, and file some quick in-browser PRs to switch people&#39;s links.</li>
</ul>


<h3>Telling search engines</h3>

<p>Use<strong><a href="https://support.google.com/webmasters/answer/139066?hl=en">canonical URLs</a></strong>to tell search engines to use the HTTPS version of your website.</p>

<pre><code>&lt;linkrel="canonical"href="https://yoursite.github.io"/&gt;
</code></pre>

<p>In Jekyll, this looks like:</p>

<pre><code>&lt;linkrel="canonical"href="{{ site.url }}{{ page.url }}"/&gt;
</code></pre>

<p>Where you&#39;ve set theurlfield in your site&#39;s_config.ymlfile. See<a href="https://github.com/18F/18f.gsa.gov/blob/b58cbcd66d2535746bfa43d42f670b9b1c105fd3/_config.yml#L26">here</a>and<a href="https://github.com/18F/18f.gsa.gov/blob/b58cbcd66d2535746bfa43d42f670b9b1c105fd3/_includes/head.html#L27">here</a>for an example.</p>

<p>(Thanks to Ylon for<a href="#comment-54c505e769702d16212a0000">suggesting this</a>!)</p>

<h3>Forcing a redirect</h3>

<p>So, how do you force a redirect for a static site when you don&#39;t control the server? You can&#39;t use a<meta refresh>tag, because you can&#39;t change what HTML gets delivered per-protocol.</p>

<p>The only choice is to<strong>do it in JavaScript</strong>. Put the following at the top of your HTML, replacingYOURDOMAINwith your user or organization name:</p>

<pre><code>varhost="YOURDOMAIN.github.io";if((host==window.location.host)&amp;&amp;(window.location.protocol!="https:"))window.location.protocol="https";
</code></pre>

<p>The above hack will immediately redirect any users who visit your domain on an insecure protocol to a secure protocol. It won&#39;t affect visitors on any other domain (likelocalhost). This is what we&#39;re doing at the<a href="https://sunlightfoundation.com">Sunlight Foundation</a>now for the documentation for our<a href="https://sunlightlabs.github.io/congress/">Sunlight Congress API</a>.</p>

<p>Make no mistake, client-side redirecting is a hack and an imperfect solution by any means. It&#39;s also not secure! But the whole point is to make sure that the links other people make to your website going<em>forward</em>are the secure kind. The more links that you and others make that go directly tohttps://, the less often the redirect will need to be triggered.</p>

<p>If you&#39;re using Jekyll, you may wish to<a href="https://github.com/sunlightlabs/congress/commit/6426761a671d46df6fc5d2526bdaf506c39d789c">follow my example</a>of using asite.enforce_sslparameter, or you can just hardcode it like above. It&#39;d be nicer if this could get baked into a Jekyll plugin, but GitHub Pages only supports a couple of whitelisted plugins.</p>

<h3>Using a custom domain with CloudFlare</h3>

<p>[<strong>Update</strong>: Rewrote this section later in 2014, after CloudFlare released their free SSL plan.]</p>

<p><a href="https://www.cloudflare.com/">CloudFlare</a>now offers<strong><a href="https://blog.cloudflare.com/introducing-universal-ssl/">free SSL termination</a></strong>for any website they host, under their "Universal SSL" plan.</p>

<p>There are a couple of caveats:</p>

<ul>
<li>The free plan uses only modern technical standards (<a href="https://blog.cloudflare.com/ecdsa-the-digital-signature-algorithm-of-a-better-internet/">ECDSA</a>,<a href="http://googleonlinesecurity.blogspot.com/2014/09/gradually-sunsetting-sha-1.html">SHA-2</a>,<a href="https://www.mnot.net/blog/2014/05/09/if_you_can_read_this_youre_sniing">SNI</a>), so customers visiting using Windows XP and/or old Internet Explorer browsers may not be able to connect.</li>
<li>Until GitHub Pages or CloudFlare fix things, you&#39;ll only be able to encrypt the connection between the user and CloudFlare. The connection between CloudFlare and GitHub Pages will need to remain in plaintext, using CloudFlare&#39;s "Flexible SSL" option. For more information on why this is the case, read<a href="https://github.com/isaacs/github/issues/156#issuecomment-57271637">these</a><a href="https://github.com/isaacs/github/issues/156#issuecomment-60453315">comments</a>that lay out how it works.</li>
</ul>


<p>If you&#39;re okay not supporting ancient clients, and with traffic flowing in clear text between CloudFlare and GitHub, you can turn on SSL for free in front of your GitHub Pages site with a custom domain.</p>

<p>But<strong>be careful</strong>. This is a tempting, but incomplete solution. I was<a href="https://github.com/MayOneUS/homepage_redesign/issues/82">very disappointed</a>to see the<a href="https://mayday.us">MayDay SuperPAC</a>use this configuration to solicit donations. An attacker who could get between CloudFlare and GitHub could have altered the form in transit to send payment information to anywhere they wanted.</p>

<p>More generally problematic is that the browser has no way of indicating to the user that the connection is not fully secure. Right now, people generally expect that if there&#39;s a lock symbol in the browser, the connection is secure between them and the website. CloudFlare&#39;s Flexible SSL configuration breaks this expectation, and there is no way from the outside to tell whether a website is using it.</p>

<blockquote><p>Gotta say, I agree with<a href="https://twitter.com/__agwa">@__agwa</a>- CloudFlare has good intentions, but their Flexible SSL dilutes the value of HTTPS:<a href="https://t.co/6B3dqMzHgK">https://t.co/6B3dqMzHgK</a>
— Eric Mill (@konklone)<a href="https://twitter.com/konklone/status/539543267311091715">December 1, 2014</a></p></blockquote>

<p>I&#39;m not saying to never use CloudFlare&#39;s Flexible SSL - but know exactly what you are doing, and communicate to CloudFlare that they need to<a href="https://twitter.com/ivanristic/status/530761077001162753">add a way for outsiders to know how secure the connection is</a>.</p>

<h3>What GitHub can do</h3>

<p>It&#39;s unlikely that GitHub is going to create the infrastructure needed to natively support custom domains, and Cloudflare&#39;s leadership will render that less of a problem.</p>

<p>However, there are still a few things GitHub can do to lower the barrier to using HTTPS and to make people more aware of it.</p>

<ul>
<li><strong>Document the feature.</strong>There&#39;s no formal announcement or description of SSL for GitHub Pages; it could disappear any time. It&#39;s worth a quick blog post and a help page, to cement GitHub&#39;s commitment and describe how to avoid common mixed content pitfalls.</li>
<li><strong>Allow SSL for custom domains configured elsewhere.</strong>As mentioned above, even if you configure your custom domain with a service like CloudFlare and point a CNAME at GitHub Pages, you&#39;ll get an error, and one that requires action on GitHub&#39;s part to fix.</li>
<li><strong>Shore up the SSL configuration.</strong>github.com&#39;s SSL is<a href="https://www.ssllabs.com/ssltest/analyze.html?d=github.com&amp;amp;s=192.30.252.128&amp;amp;hideResults=on">world-class</a>, butgithub.io&#39;s could still<a href="https://www.ssllabs.com/ssltest/analyze.html?d=sunlightlabs.github.io">use some tweaks</a>around forward secrecy and cipher choices.</li>
<li>Most importantly,<strong>let users turn HTTPS on by default</strong>, with a checkbox in their settings that forces a redirect at the server level. That would render the entire hack above unnecessary, and lead a lot more people to Just Do It. In fact, turn on the checkbox by default for new users, and for existing users who don&#39;t yet have any GitHub Pages!</li>
</ul>


<p>2014 may well be the Year of SSL, and GitHub is fertile ground for expanding the playing field. If you want to push this forward, email<a href="mailto:support@github.com">support@github.com</a>and ask for formal HTTPS support in GitHub Pages. (Referencing this blog post can&#39;t hurt.)</p>

<p>In the meantime, I&#39;ve been going around<a href="https://github.com/project-open-data/project-open-data.github.io/pull/295">filing PRs</a>and<a href="https://github.com/cfpb/cfpb.github.io/issues/22">opening tickets</a>with GitHub Pages sites I care about, to squash mixed-content warnings and ensure they&#39;re HTTPS-ready. Go adopt some sites you know on GitHub Pages and do the same!</p></div>

<!-- Handle Language Change -->
<script type="text/javascript">
    var $zh = document.querySelector(".zh");
    var $en = document.querySelector(".en");
    function onLanChange(index){
        if(index == 0){
            $zh.style.display = "block";
            $en.style.display = "none";
        }else{
            $en.style.display = "block";
            $zh.style.display = "none";
        }
    }
    onLanChange(0);
</script>

