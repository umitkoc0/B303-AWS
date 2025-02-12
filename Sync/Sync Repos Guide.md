# Sync Repos Script Kullanımı

Bu döküman, GitHub üzerindeki bir kaynaktan belirli bir hedefe dosya senkronizasyonu yapmak için kullanılacak `sync_repos.sh` scriptinin oluşturulması ve çalıştırılması sürecini adım adım açıklamaktadır.

---

## 1️⃣ Dosyayı Oluşturma

Öncelikle masaüstüne `sync_repos.sh` adında bir dosya oluşturun:

```bash
cd ~/Desktop
nano sync_repos.sh
```

---

## 2️⃣ Kodları Dosyaya Yapıştırma 

Açılan `nano` editörüne aşağıdaki kodları yapıştırın: **Önce 30. Satıra hedef reponun linkini girin. Daha sonra 23 - 86 aralığını kopyalayın**

```bash
#!/bin/bash

# Hata oluşursa işlemi durdur
set -e

# Kaynak ve hedef repo URL'leri
KAYNAK_REPO="https://github.com/techproeducation-batchs/B303-aws-devops.git"
HEDEF_REPO="SİZİN GİTHUB REPOSU URL’NİZ"

# Masaüstünde "AWS Dokuman" klasörünü kontrol et, yoksa oluştur
AWS_KLASOR=~/Desktop/AWS\ Dokuman
if [ ! -d "$AWS_KLASOR" ]; then
    mkdir -p "$AWS_KLASOR"
fi

# Geçici bir klasör oluştur ve kaynak repoyu oraya klonla
GECICI_KLASOR=$(mktemp -d)
git clone --depth 1 "$KAYNAK_REPO" "$GECICI_KLASOR"

# Geçmiş commitleri sıfırla
rm -rf "$GECICI_KLASOR/.git" && git -C "$GECICI_KLASOR" init

# README dosyasını hariç tutarak dosyaları "AWS Dokuman" klasörüne taşı (ana klasör olmadan)
cp -r "$GECICI_KLASOR"/* "$AWS_KLASOR"/

# Geçici klasörü sil
rm -rf "$GECICI_KLASOR"

# "AWS Dokuman" klasörüne gir
cd "$AWS_KLASOR"

# Eğer Git başlatılmamışsa başlat
git init

# Uzak repo bağlantısını kontrol et ve yanlışsa düzelt
MEVCUT_REMOTE=$(git remote get-url origin 2>/dev/null || echo "yok")
if [[ "$MEVCUT_REMOTE" != "$HEDEF_REPO" ]]; then
    git remote remove origin 2>/dev/null || true
    git remote add origin "$HEDEF_REPO"
fi

echo "✅ Uzak repo ayarlandı: $(git remote get-url origin)"

# Değişiklik olup olmadığını kontrol et
git add --all
git commit -m "nova files additae" || echo "❗ Commit yapılacak bir değişiklik bulunamadı."

# Güncellenmiş dosyaları önce çekip sonra push edelim
git branch -M main
git pull --rebase origin main || true

git push -u origin main --force

echo "✅ Dosya aktarımı ve gönderme işlemi başarıyla tamamlandı!"

# 🔍 **Push işlemi başarıyla yapıldı mı? Kontrol edelim**
REMOTE_KONTROL=$(git ls-remote "$HEDEF_REPO" | grep refs/heads/main)

if [ -z "$REMOTE_KONTROL" ]; then
    echo "❌ Push işlemi başarısız oldu! GitHub reposunu kontrol et."
    exit 1
else
    echo "✅ Push işlemi başarılı! Dosyalar GitHub'daki hedef repoda mevcut."
fi
```

Yapıştırdıktan sonra **CTRL + X** tuşlarına basın, ardından **Y** tuşuna basarak kaydetmeyi onaylayın ve **Enter** tuşuna basarak çıkın.

---

## 3️⃣ Çalıştırılabilir Yapma

Dosyanın çalıştırılabilir olması için aşağıdaki komutu çalıştırın:

```bash
chmod +x ~/Desktop/sync_repos.sh
```

---

## 4️⃣ Scripti Çalıştırma

Artık scripti çalıştırabilirsiniz:

```bash
~/Desktop/sync_repos.sh
```

Bu komut çalıştırıldığında, kaynak GitHub reposundaki dosyalar belirtilen hedef GitHub reposuna senkronize edilecektir.

✅ **İşlem başarıyla tamamlanacaktır!**

