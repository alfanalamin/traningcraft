## Review 2024-12-19
oleh : [Fikar](https://github.com/superpikar/)

Berikut ini bebeberapa review :
- Tentang `People`
  - List `People` pada `pageBuilder` tidak diinput 1-1, karena nantinya butuh ditampilkan di halaman lain. contoh skenario : 
    - tampilkan list `People` tim Manajemen saja di halaman /about
    - tampilkan seluruh list `People` di laman /our-team
  - Sehingga sebaiknya `People` dibuat `Section-Structure`, untuk entry nya 2 level : 
    - level 1 = nama Tim nya
    - level 2 = anggota tim nya
    - misal : 
      - Tim Manajemen
        - dr. Akbar Fahmi
        - Azis
        - Maskur Ali
      - Tim IT
        - Alpan
        - Ridho
        - Hamzah
  - Sehingga sebaiknya pada Entry Type People, fieldnya hanya Title dan `Parent People` (relasi ke entry nama tim)
    - misal pada laman /about, `pageBuilder.people` pilih `Parent People` nya `Tim Manajemen`
  - Sehingga pada /our-team : 
    - tinggal buat template/our-team.twig
    - isinya query all People
    - saat render diloop level 1 dulu, baru didapatkan child nya
- Tentang Laman Statis
  - karena isinya sama2 page builder, untuk masing2 laman statis tidak perlu dibuat single
  - bisa juga dengan approach membuat section structure Page, dan field [Template Select](https://plugins.craftcms.com/template-select?craft5=). seperti yang digunakan pada https://github.com/pikarlabs/craftcms-starterkit
  - atau dibuat fleksibel di pageBuilder dengan menambahkan entry type baru untuk menampilkan section yg dimaksud
- Tentang Page Builder
  - konsep dari page builder :
    - konten didefinisikan di admin panel
    - sedangkan templatenya akan konsisten outputnya di semua halaman yang memiliki field `pageBuilder`
    - sehingga rendering field `pageBuilder` harus dibuat component, karena reusable di semua laman. contoh implementasinya : 
    - daripada :
      ```
      // render di /index.twig
      {% for block in entry.pageBuilder %}
		{% switch block.type %}
			{% case 'contentPage' %}
            {% case 'icons' %}
            {% case 'hero' %}
            {% case 'product' %}
        {% endswitch %}
      {% endfor %}
      
      // render di /about/_entry.twig
      {% for block in entry.pageBuilder %}
		{% switch block.type %}
			{% case 'contentPage' %}
            {% case 'hero' %}
            {% case 'galery' %}
            {% case 'people' %}
        {% endswitch %}
      {% endfor %}
      ```
    - sebaiknya dijadikan reusable
      ```
      // render di /_components/page_builder.twig
      {% set entry = entry ?? [] %}
      {% for block in entry.pageBuilder %}
        {% switch block.type %}
            {% case 'contentPage' %}
            {% case 'icons' %}
            {% case 'hero' %}
            {% case 'product' %}
            {% case 'galery' %}
            {% case 'people' %}
        {% endswitch %}
      {% endfor %}
      
      // render di /index.twig
      {% include "_components/page_builder.twig" with {
        entry: entry,
      } %}
      
      // render di /page/_entry.twig
      {% include "_components/page_builder.twig" with {
        entry: entry,
      } %}
      ```
