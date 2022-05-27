<h1> Selamlar, Aptos testneti i&ccedil;in Node kurma rehberi ile karşınızdayım, formu doldurmayı unutmayın. Sunucunuzu temin edip SSH ile baglandıktan sonra y&ouml;nergeleri takip edin.</h1>
<p>https://community.aptoslabs.com/ adresinden Form doldurmanız lazım. Siteye girin, Github ile giriş yapın sonrasında mail isteyecektir mailinizi girin.</p>
<p>Maile gelen linkten forma devam etmemiz lazım.</p>
<p><br></p>
<p>Linke girdiginizde istenen teknik bilgileri node kurulumu tamamlandıktan sonra...</p>
<hr>
<p>source ~/.bash_profile</p>
<p>cat ~/.aptos/$APTOS_NODENAME.yaml</p>
<hr>
<p><br></p>
<p>Bu komutları yazarak alabilirsiniz doldurup KYC Yapmanız gerekecetir.s</p>
<p><br></p>
<p>Tanıtıma vs d&uuml;şebilir Aptos gelen maile girin ve Verify Mail basın</p>
<p><br></p>
<p>Sistem Gereksinimleri</p>
<p><br></p>
<p>İşletim Sistemi Ubuntu 20.04</p>
<p>CPU: 4 cores&nbsp;</p>
<p>Memory: 8GB Ram</p>
<p><br></p>
<p><br></p>
<p>Kurulum Komutları</p>
<p><br></p>
<p>cd</p>
<p>sudo apt install screen</p>
<p>screen -S node</p>
<p>wget -q -O aptosch23.sh https://databox.acadao.org/aptosch23.sh &amp;&amp; chmod +x aptosch23.sh &amp;&amp; sudo /bin/bash aptosch23.sh</p>
<p>docker logs -f --tail 100 aptos-validator-1</p>
<p><br></p>
<p>------------</p>
<p>Kurulum otomatik Başlıcaktır, Y&ouml;nergeler ile sizden ne yapmanız gerektigini s&ouml;yleyecek</p>
<p>Daha &ouml;nce kurduysanız otomatik g&uuml;nceller, ilk efa kuruyorsanız c&uuml;zdan bilginizi not edin.</p>
<p>ardından CTRL+A+D ile node&apos;unuzu kapatabilirsiniz</p>
<p><br></p>
<p>Yararlı komutlar</p>
<hr>
<p>Node Durdurma</p>
<p>docker logs -f --tail 100 aptos-validator-1</p>
<p>Reet Atma</p>
<p>cd ~/.aptos &amp;&amp; docker-compose restart</p>
<p>Ağdaki blogu bakma</p>
<p>curl 127.0.0.1:9101/metrics 2&gt; /dev/null | grep aptos_state_sync_version</p>
<p><br></p>
<p><br></p>
<p>Kullanılan portlar</p>
<p>TCP ports: 80, 8080, 6180, 6181, 6182, 9101, 9103</p>
<p><br></p>
