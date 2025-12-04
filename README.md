import hashlib

class URLShortener:
    def __init__(self):
        self.short_to_long = {}   # kode pendek menjadi URL asli
        self.long_to_short = {}   # URL asli menjadi kode pendek
        self.domain = "https://mini.ly/"

    def shorten(self, long_url):
        # Jika URL sudah pernah disingkat
        if long_url in self.long_to_short:
            code = self.long_to_short[long_url]
            return self.domain + code

        # Buat hash SHA-1
        hash_hex = hashlib.sha1(long_url.encode()).hexdigest()

        # Gunakan 7 karakter pertama sebagai kode
        code = hash_hex[:7]

        # Jika terjadi tabrakan, tambahkan panjang kode
        i = 7
        while code in self.short_to_long:
            code = hash_hex[:i]
            i += 1

        # Simpan ke hash map
        self.short_to_long[code] = long_url
        self.long_to_short[long_url] = code

        return self.domain + code

    def expand(self, short_url):
        code = short_url.replace(self.domain, "")
        return self.short_to_long.get(code, "URL tidak ditemukan")

#==========================================
#-----Program Utama Dengan Input User-----
#==========================================

shortener = URLShortener()

print("=== URL SHORTENER (Hashing Map) ===")

while True:
    print("\nMenu:")
    print("1. Shorten URL (memendekkan URL)")
    print("2. Expand URL (mengembalikan URL asli)")
    print("3. Keluar")

    pilihan = input("Pilih menu (1/2/3): ")

    if pilihan == "1":
        url = input("Masukkan URL panjang: ")
        short_url = shortener.shorten(url)
        print("URL pendek:", short_url)

    elif pilihan == "2":
        short = input("Masukkan URL pendek: ")
        original = shortener.expand(short)
        print("URL asli:", original)

    elif pilihan == "3":
        print("PROGRAM SELESAI.")
        break

    else:
        print("Pilihan TIDAK VALID!")
