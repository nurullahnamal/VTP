# VTP Modları ve Konfigürasyonu (Cisco Switch)<br>

VTP Nedir, Ne İşe Yarar ve Hangi Mod Ne Amaçla Kullanılır? <br>

Büyük ağ yapılarında, birden fazla switch’in bulunduğu ortamlarda VLAN yönetimini tek tek her switch üzerinde yapmak hem zaman alıcıdır hem de hata riskini artırır. Bu sorunu çözmek için Cisco’nun sunduğu VTP (VLAN Trunking Protocol) protokolü devreye girer.
VTP sayesinde, VLAN’lar merkezi bir switch üzerinden tanımlanır ve trunk bağlantılar aracılığıyla diğer switch’lere otomatik olarak iletilir. Bu yapı, ağ yöneticisine merkezi, kolay ve hızlı VLAN yönetimi sağlar. <br>


VTP Bize Ne Gibi Kolaylıklar Sağlar? <br>
1. Merkezi VLAN Yönetimi <br>
VLAN’ları tek bir yerden (VTP Server’dan) tanımlayarak tüm switch’lere otomatik olarak dağıtabilirsin. <br>
Her switch’te tek tek VLAN oluşturma ihtiyacını ortadan kaldırır.<br>
2. Zaman ve İş Gücü Tasarrufu<br>
Büyük ağlarda yüzlerce VLAN’ın manuel tanımlanması zaman alır. VTP ile bu işlem çok daha hızlı ve otomatiktir.<br>
3. Hata Riskini Azaltır<br>
Her switch’te manuel VLAN tanımı yaparken oluşabilecek isim/numara hatalarının önüne geçilmiş olur.<br>
Tutarlılık sağlar.<br>
4. Otomatik Senkronizasyon<br>
VLAN bilgileri versiyon numarası (revision number) ile eşleştirilir. Yeni bilgiler otomatik olarak diğer switch’lere aktarılır.<br>
5. Trunk Bağlantılarda VLAN Bilgileri Paylaşılır<br>
Switch’ler arasında trunk bağlantı varsa, bu bağlantılar üzerinden VLAN bilgileri kolayca paylaşılır.<br>
6. Büyük Kurumsal Ağlarda Ölçeklenebilirlik<br>
Özellikle çok sayıda switch içeren yapılarda yapılandırma ve yönetimi oldukça kolaylaştırır.<br>


Hangi Mod Ne Zaman Seçilmeli?<br>

🔹 Server: Ağın merkezi noktası, VLAN’ların yönetildiği yer.<br>

🔹 Client: Sadece VLAN’ları kullanacak uç switchler.<br>

🔹 Transparent: Ayrı bir yapı kurmak istenen veya VLAN bilgisinin yayılmasını istemediğin özel switchler. <br>

Şimdi ise yapılandırma adımlarını inceleyelim.<br>

VTP Server Switch Konfigürasyonu (Backbone Switch)<br>
enable                      # Yetkili (privileged) moda geçiş <br>
configure terminal          # Global konfigürasyon moduna giriş <br>
vtp mode server             # VTP modunu "Server" olarak ayarla <br>
vtp domain ccna             # VTP domain adını "ccna" olarak ayarla <br>
vtp password 1234           # VTP şifresini "1234" olarak ayarla <br>

VLAN Tanımları <br>
vlan 10                     # VLAN 10’u oluştur  <br>
 name data1                 # VLAN 10’a "data1" ismini ver  <br>
vlan 20 <br>
 name data2 <br>
vlan 30 <br>
 name data3 <br>
vlan 40 <br>
 name data4 <br>
vlan 50 <br>
 name data5 <br>

oluşturulan vlanlar <br>
Trunk Port Ayarı <br>
interface gigabitEthernet 0/1   # G0/1 arayüzünü seç <br>
 switchport mode trunk          # Arayüzü trunk moda al (VLAN geçişine izin verir) <br>
Kontrol <br>
show vtp status             # VTP mod, domain, revision gibi bilgileri gösterir <br>
show interface trunk        # Trunk port durumlarını ve VLAN'ları listeler <br>
Transparent Mod Switch Konfigürasyonu <br>
enable <br>
configure terminal <br>
vtp mode transparent        # VTP modunu "transparent" yap (VLAN iletmez/almaz, sadece lokal işler) <br>
vtp domain ccna             # Domain adı "ccna" ile aynı olmalı (şifreyle birlikte eşleşmeli) <br>
vtp password 1234           # VTP şifresi <br>
Trunk Port Ayarı <br>
interface gigabitEthernet 0/1 <br>
 switchport mode trunk          # Trunk port olarak ayarlanır <br>
 <br>
transparent mod da vlanları göremiyoruz. <br>
Client Portlarını Trunk Moda Alma: <br>
interface fastEthernet 0/1 <br>
 switchport mode trunk          # Client 1’e giden portu trunk yap <br>
 <br>
interface fastEthernet 0/2
 switchport mode trunk          # Client 2’ye giden portu trunk yap <br>
 <br>
interface fastEthernet 0/3
 switchport mode trunk          # Client 3’e giden portu trunk yap <br>
Client Switch 1 Konfigürasyonu <br>
enable <br>
configure terminal <br>
vtp mode client             # VTP modunu "client" olarak ayarla <br>
vtp domain ccna             # Domain adını "ccna" olarak ayarla  <br>
vtp password 1234           # Şifreyi "1234" olarak ayarla <br>
 <br>
interface fastEthernet 0/1 <br>
 switchport mode trunk      # Trunk bağlantı yapılacak port <br>
 <br>
end <br>
show vtp status             # VTP bilgilerini kontrol et <br>
 <br>
client moda aldığımız hali <br>
Client Switch 2 Konfigürasyonu <br>
enable <br>
configure terminal <br>
vtp mode client <br>
vtp domain ccna <br>
vtp password 1234 <br>
 <br>
interface fastEthernet 0/1 <br>
 switchport mode trunk <br>

end <br>
show vtp status <br>
Client Switch 3 Konfigürasyonu <br>
enable <br>
configure terminal <br>
vtp mode client <br>
vtp domain ccna <br>
vtp password 1234 <br>
 <br>
interface fastEthernet 0/1 <br>
 switchport mode trunk <br>

end <br>
show vtp status <br>


Test 192.168.10.10 dan 192.168.10.100 ping atma <br>
Dikkat Edilmesi Gerekenler <br>
Yanlışlıkla başka bir switch’i server modda ağa dahil etmek tüm ağı etkileyebilir. <br>
Revision Number yüksek bir switch, yanlış VLAN’ları tüm ağa yayabilir. <br>
👉 Bu yüzden “transparent” mod” ya da revision reset dikkatle yapılmalı <br>
Kaynaklar : Cem Bayraktaroğlu CCNA eğitimi <br>

Linkedin: https://www.linkedin.com/in/nurullahnamal/ <br>

Github: github.com/nurullahnamal
