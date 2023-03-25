# 前瞻性法医数据科学——有可能预测犯罪并预防犯罪吗？

> 原文：<https://towardsdatascience.com/prospective-forensic-data-science-is-it-possible-to-predict-a-crime-and-prevent-it-28b48ba19825?source=collection_archive---------8----------------------->

社交网络中的数字足迹已经成为寻找罪犯的信息来源。例如，在巴西，一些与种族主义有关的案件由于 FaceBook (FB)上的种族主义帖子而受到指使。例如，2015 年 10 月，非裔巴西电视和电影明星 [Tais Araújo](https://en.wikipedia.org/wiki/Ta%C3%ADs_Ara%C3%BAjo#External_links) *因其肤色和发型而成为 facebook 上[种族主义评论的受害者。结果，大约有](http://g1.globo.com/rio-de-janeiro/noticia/2015/11/atriz-tais-araujo-e-alvo-de-comentarios-racistas-em-rede-social.html) [30 人根据他们的 FB 档案被调查](https://tecnologia.uol.com.br/noticias/redacao/2015/11/04/cerca-de-30-perfis-do-facebook-serao-investigados-por-ofensas-a-tais-araujo.htm)，其中[五人因为他们的职位被逮捕](http://veja.abril.com.br/entretenimento/cinco-suspeitos-sao-detidos-por-ataque-racista-a-tais-araujo/)。与那些罪犯所想的相反，远程计算机提供的明显的匿名性是他们犯罪的证据。数字足迹的好用途！

此外，数字足迹已经被用来防止潜在的更有害的犯罪。在里约热内卢举办夏季奥运会之前，[巴西联邦警察](https://en.wikipedia.org/wiki/Federal_Police_of_Brazil)发现了[一个据称与 ISIS 有关联的组织，该组织正计划对奥运设施进行恐怖袭击](http://epoca.globo.com/tempo/noticia/2016/07/pf-prende-celula-do-estado-islamico-que-planejava-atentado-na-rio-2016.html)。该组织利用脸书和推特赞美 ISIS，庆祝世界各地的恐怖袭击。他们还使用 WhatsApp 和 Telegram 等即时通讯应用程序来交换信息和策划攻击。联邦警察切断了他们的通讯，暴露了他们的计划，这足以将他们的行为定性为犯罪。

此类犯罪引发了另一个问题:即时通讯服务提供商是否应该为了与当局合作而中断用户对话？[1988 年《巴西宪法》(第 5 条第十二款)](https://www.constituteproject.org/constitution/Brazil_2014.pdf)规定，通信保密不可侵犯，除非法官作为一项非常措施并在法律规定的情况下下令保密。当然，这种数字通信在巴西宪法颁布时并不存在，但最高法院将这种豁免权延伸到了这些新技术。2016 年 5 月[发生了一件不寻常的案件](http://www1.folha.uol.com.br/mercado/2016/05/1766886-whatsapp-sai-do-ar-por-72-horas-no-brasil-por-determinacao-da-justica.shtml)，当时联邦警察正在调查毒品生产商和经销商。一名来自一个小镇的联邦法官下令 WhatsApp 的所有者 FaceBook 披露嫌疑人的信息。脸书拒绝这样做，声称他们没有这个信息了。由于拒绝，这位法官下令 WhatsApp 必须在巴西全境离线 72 小时。由于这款应用在巴西被广泛使用，包括用于商业，整个巴西都因为这一法令而变得疯狂。该判决仅执行了几个小时，因为该判决被更高一级法院推翻。

事实是，FaceBook 有权访问 WhatsApp 用户的对话内容。用户协议明确说明了。该内容用于向用户投放广告。问题是:如果当局要求，FaceBook 和其他即时通讯服务提供商应该存储他们的用户对话并披露吗？或者至少受到公正的对待？

然而，这段历史可以有另一个篇章。如果这些即时通讯服务可以访问用户的聊天内容，并通过文本分析了解他们，那么在一些可控和合法的情况下，当有强有力的证据表明他们的用户正在犯罪时，他们是否应该向当局发出警报？例如，算法可以被训练来识别婴儿色情图像。当一个人向另一个人发送这种图像时，他们应该被用来识别这种犯罪吗？**文本分析可用于理解关于女性奴役的对话。为了发现这种罪行而中断交流并可能暴露数百万人，这样做值得吗？此外，这些算法可以被训练来识别这种犯罪何时仍在计划中。当一个潜在的罪犯将要犯罪时，即时通讯提供商能警告政策吗？

未来的法医数据科学可以实现这种打击/预防犯罪的场景，这类似于汤姆·克鲁斯的电影[少数派报告](https://en.wikipedia.org/wiki/Minority_Report_(film))。但是，正如电影中一样，预测算法可能会出错或有偏差(有意或无意)。有了这个，一个无辜的生命可能就毁了。嗯……从这个可能的世界中，大量的伦理问题涌现出来。然而，这不是本文的主题，这些问题可以在其他时间出现。

*葡萄牙语参考文献

* *发送这种图像的简单行为并不意味着犯罪。例如，心理学家可能会将这样的图像发送给另一个人，以帮助解决问题。