# React Native + Expo: Hava Durumu Uygulaması Dersi

## Ders Genel Bakış
Bu derste, hava durumu temalı bir mobil uygulama geliştirerek React Native'de **StyleSheet**, **useState**, **ScrollView** ve **navigasyon** kavramlarını öğreneceğiz.

### Öğreneceğiniz Konular
- ✅ StyleSheet ile profesyonel stil yönetimi (merkezi odak)
- ✅ useState ile durum yönetimi
- ✅ ScrollView ile kaydırılabilir içerik
- ✅ BottomTabNavigator ile ekranlar arası geçiş
- ✅ Çok ekranlı uygulama yapısı

---

## Bölüm 1: Proje Kurulumu ve Hazırlık

### Adım 1.1: Yeni Expo Projesi Oluşturma

Terminal veya komut satırında şu komutu çalıştırın:

```bash
npx create-expo-app@latest hava-durumu-app
cd hava-durumu-app
```

### Adım 1.2: Gerekli Paketleri Yükleme

Navigasyon için gerekli paketleri yükleyelim:

```bash
npx expo install @react-navigation/native @react-navigation/bottom-tabs
npx expo install react-native-screens react-native-safe-area-context
```

### Adım 1.3: Proje Yapısı

Aşağıdaki klasör yapısını oluşturun:

```
hava-durumu-app/
├── app/
│   ├── (tabs)/
│   │   ├── _layout.tsx
│   │   ├── index.tsx
│   │   ├── arama.tsx
│   │   └── favoriler.tsx
│   └── _layout.tsx
├── components/
└── assets/
```

---

## Bölüm 2: StyleSheet - Merkezi Odak Noktası

### 2.1: StyleSheet Nedir ve Neden Kullanırız?

**StyleSheet**, React Native'de stilleri organize etmenin ve performanslı bir şekilde yönetmenin yoludur. İnline stiller yerine StyleSheet kullanmak:
- ✅ Kodun daha okunabilir olmasını sağlar
- ✅ Stilleri tek bir yerde toplar
- ✅ Performans optimizasyonu sağlar
- ✅ Hataları daha kolay yakalar

### 2.2: Temel StyleSheet Kullanımı

Basit bir örnekle başlayalım:

```tsx
import { View, Text, StyleSheet } from 'react-native';

export default function OrnekEkran() {
  return (
    <View style={styles.container}>
      <Text style={styles.baslik}>Hava Durumu</Text>
      <Text style={styles.altBaslik}>Şehrinizi seçin</Text>
    </View>
  );
}

// StyleSheet ile stiller tanımlanır
const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#E3F2FD',
    padding: 20,
  },
  baslik: {
    fontSize: 28,
    fontWeight: 'bold',
    color: '#1976D2',
    marginBottom: 10,
  },
  altBaslik: {
    fontSize: 16,
    color: '#424242',
  },
});
```

**Önemli Noktalar:**
- `StyleSheet.create()` ile stil objesi oluşturulur
- Her stil bir isimle tanımlanır (container, baslik, altBaslik)
- `style={styles.stilAdi}` şeklinde kullanılır

### 2.3: Birden Fazla Stil Birleştirme

Bazen bir component'e birden fazla stil uygulamak isteriz:

```tsx
<Text style={[styles.temelMetin, styles.vurgu]}>Önemli Bilgi</Text>

const styles = StyleSheet.create({
  temelMetin: {
    fontSize: 16,
    color: '#000000',
  },
  vurgu: {
    fontWeight: 'bold',
    backgroundColor: '#FFEB3B',
    padding: 5,
  },
});
```

**Array kullanarak stiller birleştirilir:** `style={[stil1, stil2, stil3]}`

### 2.4: Koşullu Stil Uygulama

Duruma göre farklı stiller uygulamak:

```tsx
<View style={[
  styles.kartKutu,
  sicaklik > 25 ? styles.sicak : styles.soguk
]}>
  <Text>{sicaklik}°C</Text>
</View>

const styles = StyleSheet.create({
  kartKutu: {
    padding: 15,
    borderRadius: 10,
    margin: 10,
  },
  sicak: {
    backgroundColor: '#FF5722',
  },
  soguk: {
    backgroundColor: '#2196F3',
  },
});
```

### 2.5: Gelişmiş StyleSheet Örneği - Hava Durumu Kartı

Şimdi daha karmaşık bir örnek yapalım:

```tsx
import { View, Text, StyleSheet } from 'react-native';

function HavaDurumuKarti({ sehir, sicaklik, durum }: any) {
  return (
    <View style={styles.kart}>
      {/* Üst Kısım */}
      <View style={styles.kartUst}>
        <Text style={styles.sehirAdi}>{sehir}</Text>
        <Text style={styles.tarih}>Bugün</Text>
      </View>
      
      {/* Orta Kısım - Ana Bilgi */}
      <View style={styles.kartOrta}>
        <Text style={styles.sicaklikBuyuk}>{sicaklik}°</Text>
        <Text style={styles.durumMetin}>{durum}</Text>
      </View>
      
      {/* Alt Kısım - Detaylar */}
      <View style={styles.kartAlt}>
        <View style={styles.detayKutu}>
          <Text style={styles.detayBaslik}>Nem</Text>
          <Text style={styles.detayDeger}>%65</Text>
        </View>
        <View style={styles.detayKutu}>
          <Text style={styles.detayBaslik}>Rüzgar</Text>
          <Text style={styles.detayDeger}>15 km/s</Text>
        </View>
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  // Ana kart container
  kart: {
    backgroundColor: '#FFFFFF',
    borderRadius: 20,
    padding: 20,
    margin: 15,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.1,
    shadowRadius: 8,
    elevation: 5, // Android için gölge
  },
  
  // Üst kısım stilleri
  kartUst: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginBottom: 15,
    borderBottomWidth: 1,
    borderBottomColor: '#E0E0E0',
    paddingBottom: 10,
  },
  sehirAdi: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#1976D2',
  },
  tarih: {
    fontSize: 14,
    color: '#757575',
  },
  
  // Orta kısım stilleri
  kartOrta: {
    alignItems: 'center',
    paddingVertical: 20,
  },
  sicaklikBuyuk: {
    fontSize: 72,
    fontWeight: '200',
    color: '#FF5722',
  },
  durumMetin: {
    fontSize: 20,
    color: '#424242',
    marginTop: 10,
  },
  
  // Alt kısım stilleri
  kartAlt: {
    flexDirection: 'row',
    justifyContent: 'space-around',
    marginTop: 15,
    borderTopWidth: 1,
    borderTopColor: '#E0E0E0',
    paddingTop: 15,
  },
  detayKutu: {
    alignItems: 'center',
  },
  detayBaslik: {
    fontSize: 12,
    color: '#757575',
    marginBottom: 5,
  },
  detayDeger: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#424242',
  },
});

export default HavaDurumuKarti;
```

**Bu Örnekte Öğrendiklerimiz:**
- 📦 Componentleri mantıksal bölümlere ayırma (üst, orta, alt)
- 🎨 flexDirection ile yatay/dikey düzenleme
- 📏 margin, padding ile boşluklar
- 🎯 borderRadius ile yuvarlatılmış köşeler
- 🌑 shadowColor, elevation ile gölgeler
- 🔤 fontSize, fontWeight ile tipografi

---

## Bölüm 3: useState ile Durum Yönetimi

### 3.1: useState Nedir?

**useState**, component içinde değişebilen veriyi (state/durum) saklamanın yoludur. Kullanıcı bir şey yazdığında, butona bastığında veya seçim yaptığında bu değişikliği takip etmek için useState kullanırız.

**Temel Sözdizimi:**
```tsx
const [deger, setDeger] = useState(baslangicDegeri);
```

- `deger`: Şu anki değer
- `setDeger`: Değeri değiştiren fonksiyon
- `baslangicDegeri`: İlk değer

### 3.2: Basit useState Örneği

```tsx
import { View, Text, Button, StyleSheet } from 'react-native';
import { useState } from 'react';

export default function SayacOrnek() {
  // Sayaç için state tanımlıyoruz, başlangıç değeri 0
  const [sayac, setSayac] = useState(0);
  
  return (
    <View style={styles.container}>
      <Text style={styles.metin}>Sayaç: {sayac}</Text>
      
      <Button 
        title="Artır" 
        onPress={() => setSayac(sayac + 1)}
      />
      
      <Button 
        title="Azalt" 
        onPress={() => setSayac(sayac - 1)}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    gap: 20, // Elemanlar arası boşluk
  },
  metin: {
    fontSize: 24,
    fontWeight: 'bold',
  },
});
```

### 3.3: Metin Girişi için useState

Hava durumu uygulamamız için şehir adı tutacağız:

```tsx
import { View, Text, TextInput, StyleSheet } from 'react-native';
import { useState } from 'react';

export default function SehirGiris() {
  // Şehir adı için state
  const [sehirAdi, setSehirAdi] = useState('');
  
  return (
    <View style={styles.container}>
      <Text style={styles.baslik}>Şehir Ara</Text>
      
      <TextInput
        style={styles.input}
        placeholder="Şehir adı girin..."
        value={sehirAdi}
        onChangeText={setSehirAdi} // Kullanıcı her harf yazdığında state güncellenir
      />
      
      <Text style={styles.bilgi}>
        Girilen şehir: {sehirAdi || 'Henüz girilmedi'}
      </Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    backgroundColor: '#F5F5F5',
  },
  baslik: {
    fontSize: 28,
    fontWeight: 'bold',
    color: '#1976D2',
    marginBottom: 20,
  },
  input: {
    backgroundColor: '#FFFFFF',
    borderWidth: 2,
    borderColor: '#2196F3',
    borderRadius: 10,
    padding: 15,
    fontSize: 18,
    marginBottom: 15,
  },
  bilgi: {
    fontSize: 16,
    color: '#424242',
  },
});
```

**Önemli:** `value={sehirAdi}` ve `onChangeText={setSehirAdi}` birlikte kullanılmalıdır. Bu, input'un "kontrollü" olmasını sağlar.

### 3.4: Karmaşık State Örneği - Hava Durumu Verisi

Birden fazla bilgiyi bir obje olarak tutmak:

```tsx
import { View, Text, Button, StyleSheet } from 'react-native';
import { useState } from 'react';

export default function HavaDurumuDetay() {
  // Obje olarak state tanımlıyoruz
  const [havaDurumu, setHavaDurumu] = useState({
    sehir: 'İstanbul',
    sicaklik: 22,
    durum: 'Güneşli',
    nem: 65,
    ruzgar: 15,
  });
  
  // Rastgele hava durumu değiştir
  const havayiDegistir = () => {
    const sehirler = ['İstanbul', 'Ankara', 'İzmir', 'Antalya'];
    const durumlar = ['Güneşli', 'Bulutlu', 'Yağmurlu', 'Karlı'];
    
    setHavaDurumu({
      sehir: sehirler[Math.floor(Math.random() * sehirler.length)],
      sicaklik: Math.floor(Math.random() * 30) + 10,
      durum: durumlar[Math.floor(Math.random() * durumlar.length)],
      nem: Math.floor(Math.random() * 40) + 40,
      ruzgar: Math.floor(Math.random() * 20) + 5,
    });
  };
  
  return (
    <View style={styles.container}>
      <View style={styles.kart}>
        <Text style={styles.sehir}>{havaDurumu.sehir}</Text>
        <Text style={styles.sicaklik}>{havaDurumu.sicaklik}°C</Text>
        <Text style={styles.durum}>{havaDurumu.durum}</Text>
        
        <View style={styles.detaylar}>
          <Text style={styles.detay}>Nem: %{havaDurumu.nem}</Text>
          <Text style={styles.detay}>Rüzgar: {havaDurumu.ruzgar} km/s</Text>
        </View>
      </View>
      
      <Button title="Şehri Değiştir" onPress={havayiDegistir} />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#E3F2FD',
    padding: 20,
  },
  kart: {
    backgroundColor: '#FFFFFF',
    borderRadius: 20,
    padding: 30,
    width: '100%',
    alignItems: 'center',
    marginBottom: 20,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.1,
    shadowRadius: 8,
    elevation: 5,
  },
  sehir: {
    fontSize: 32,
    fontWeight: 'bold',
    color: '#1976D2',
    marginBottom: 10,
  },
  sicaklik: {
    fontSize: 64,
    fontWeight: '200',
    color: '#FF5722',
    marginVertical: 10,
  },
  durum: {
    fontSize: 24,
    color: '#424242',
    marginBottom: 20,
  },
  detaylar: {
    flexDirection: 'row',
    gap: 30,
    borderTopWidth: 1,
    borderTopColor: '#E0E0E0',
    paddingTop: 15,
  },
  detay: {
    fontSize: 16,
    color: '#757575',
  },
});
```

---

## Bölüm 4: ScrollView ile Kaydırılabilir İçerik

### 4.1: ScrollView Nedir ve Ne Zaman Kullanılır?

**ScrollView**, içerik ekrana sığmadığında kaydırma yapılabilmesini sağlar. Özellikle:
- 📜 Uzun liste veya içerikler için
- 📱 Küçük ekranlarda çok bilgi gösterirken
- 🔄 Yatay kaydırma (carousel) için

**Temel kullanım:**
```tsx
import { ScrollView } from 'react-native';

<ScrollView>
  {/* Buraya istediğiniz kadar içerik koyabilirsiniz */}
</ScrollView>
```

### 4.2: Dikey ScrollView Örneği

```tsx
import { ScrollView, View, Text, StyleSheet } from 'react-native';

export default function HavaDurumuListesi() {
  // Sahte hava durumu verisi
  const sehirler = [
    { id: 1, sehir: 'İstanbul', sicaklik: 22, durum: 'Güneşli' },
    { id: 2, sehir: 'Ankara', sicaklik: 18, durum: 'Bulutlu' },
    { id: 3, sehir: 'İzmir', sicaklik: 26, durum: 'Güneşli' },
    { id: 4, sehir: 'Antalya', sicaklik: 28, durum: 'Açık' },
    { id: 5, sehir: 'Bursa', sicaklik: 20, durum: 'Yağmurlu' },
    { id: 6, sehir: 'Adana', sicaklik: 30, durum: 'Sıcak' },
  ];
  
  return (
    <ScrollView style={styles.scrollContainer}>
      <View style={styles.icerik}>
        <Text style={styles.baslik}>Şehirler</Text>
        
        {sehirler.map((item) => (
          <View key={item.id} style={styles.kartKutu}>
            <Text style={styles.kartSehir}>{item.sehir}</Text>
            <Text style={styles.kartSicaklik}>{item.sicaklik}°C</Text>
            <Text style={styles.kartDurum}>{item.durum}</Text>
          </View>
        ))}
      </View>
    </ScrollView>
  );
}

const styles = StyleSheet.create({
  scrollContainer: {
    flex: 1,
    backgroundColor: '#F5F5F5',
  },
  icerik: {
    padding: 20,
  },
  baslik: {
    fontSize: 28,
    fontWeight: 'bold',
    color: '#1976D2',
    marginBottom: 20,
  },
  kartKutu: {
    backgroundColor: '#FFFFFF',
    borderRadius: 15,
    padding: 20,
    marginBottom: 15,
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 1 },
    shadowOpacity: 0.1,
    shadowRadius: 4,
    elevation: 3,
  },
  kartSehir: {
    fontSize: 20,
    fontWeight: 'bold',
    color: '#424242',
    flex: 1,
  },
  kartSicaklik: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#FF5722',
    marginRight: 10,
  },
  kartDurum: {
    fontSize: 14,
    color: '#757575',
  },
});
```

### 4.3: Yatay ScrollView - Günlük Tahmin

```tsx
import { ScrollView, View, Text, StyleSheet } from 'react-native';

export default function GunlukTahmin() {
  const gunler = [
    { gun: 'Pzt', sicaklik: 22, durum: '☀️' },
    { gun: 'Sal', sicaklik: 20, durum: '⛅' },
    { gun: 'Çar', sicaklik: 18, durum: '🌧️' },
    { gun: 'Per', sicaklik: 19, durum: '⛅' },
    { gun: 'Cum', sicaklik: 23, durum: '☀️' },
    { gun: 'Cmt', sicaklik: 25, durum: '☀️' },
    { gun: 'Paz', sicaklik: 24, durum: '⛅' },
  ];
  
  return (
    <View style={styles.container}>
      <Text style={styles.baslik}>7 Günlük Tahmin</Text>
      
      {/* Yatay kaydırma için horizontal={true} */}
      <ScrollView 
        horizontal={true}
        showsHorizontalScrollIndicator={false} // Kaydırma çubuğunu gizle
        style={styles.yatayScroll}
      >
        {gunler.map((item, index) => (
          <View key={index} style={styles.gunKutu}>
            <Text style={styles.gunAdi}>{item.gun}</Text>
            <Text style={styles.emoji}>{item.durum}</Text>
            <Text style={styles.sicaklik}>{item.sicaklik}°</Text>
          </View>
        ))}
      </ScrollView>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    padding: 20,
    backgroundColor: '#FFFFFF',
  },
  baslik: {
    fontSize: 20,
    fontWeight: 'bold',
    color: '#1976D2',
    marginBottom: 15,
  },
  yatayScroll: {
    flexDirection: 'row',
  },
  gunKutu: {
    backgroundColor: '#E3F2FD',
    borderRadius: 15,
    padding: 15,
    marginRight: 10,
    alignItems: 'center',
    width: 80,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 1 },
    shadowOpacity: 0.1,
    shadowRadius: 3,
    elevation: 2,
  },
  gunAdi: {
    fontSize: 14,
    color: '#424242',
    fontWeight: 'bold',
    marginBottom: 8,
  },
  emoji: {
    fontSize: 32,
    marginVertical: 5,
  },
  sicaklik: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#1976D2',
    marginTop: 5,
  },
});
```

**ScrollView Özellikleri:**
- `horizontal={true}`: Yatay kaydırma
- `showsHorizontalScrollIndicator={false}`: Kaydırma çubuğunu gizler
- `showsVerticalScrollIndicator={false}`: Dikey çubuğu gizler
- `contentContainerStyle`: İçeriğe stil vermek için

---

## Bölüm 5: BottomTabNavigator ile Ekranlar Arası Geçiş

### 5.1: Navigasyon Nedir?

**Navigasyon**, uygulamanızda farklı ekranlar arasında geçiş yapmayı sağlar. Örneğin:
- 🏠 Ana Sayfa
- 🔍 Arama
- ⭐ Favoriler

### 5.2: Expo Router ile Tab Navigasyon Kurulumu

Expo Router kullanarak tab navigasyon kuracağız.

**app/_layout.tsx** (Ana Layout)

```tsx
import { Stack } from 'expo-router';

export default function RootLayout() {
  return <Stack screenOptions={{ headerShown: false }} />;
}
```

**app/(tabs)/_layout.tsx** (Tab Navigasyon Yapısı)

```tsx
import { Tabs } from 'expo-router';
import { StyleSheet } from 'react-native';

export default function TabLayout() {
  return (
    <Tabs
      screenOptions={{
        tabBarActiveTintColor: '#1976D2', // Aktif tab rengi
        tabBarInactiveTintColor: '#757575', // Pasif tab rengi
        tabBarStyle: styles.tabBar,
        tabBarLabelStyle: styles.tabBarLabel,
        headerStyle: styles.header,
        headerTintColor: '#FFFFFF',
        headerTitleStyle: styles.headerTitle,
      }}
    >
      <Tabs.Screen
        name="index"
        options={{
          title: 'Ana Sayfa',
          tabBarIcon: () => '🏠',
          headerTitle: 'Hava Durumu',
        }}
      />
      <Tabs.Screen
        name="arama"
        options={{
          title: 'Arama',
          tabBarIcon: () => '🔍',
          headerTitle: 'Şehir Ara',
        }}
      />
      <Tabs.Screen
        name="favoriler"
        options={{
          title: 'Favoriler',
          tabBarIcon: () => '⭐',
          headerTitle: 'Favori Şehirler',
        }}
      />
    </Tabs>
  );
}

const styles = StyleSheet.create({
  tabBar: {
    backgroundColor: '#FFFFFF',
    borderTopWidth: 1,
    borderTopColor: '#E0E0E0',
    height: 60,
    paddingBottom: 8,
    paddingTop: 8,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: -2 },
    shadowOpacity: 0.1,
    shadowRadius: 4,
    elevation: 8,
  },
  tabBarLabel: {
    fontSize: 12,
    fontWeight: '600',
  },
  header: {
    backgroundColor: '#1976D2',
  },
  headerTitle: {
    fontWeight: 'bold',
    fontSize: 20,
  },
});
```

**Tab Bar Stilleri Açıklaması:**
- `tabBarActiveTintColor`: Seçili tab'ın rengi
- `tabBarInactiveTintColor`: Seçili olmayan tab'ların rengi
- `tabBarStyle`: Tab bar'ın genel stili
- `headerStyle`: Üst başlık stili

---

## Bölüm 6: Tüm Ekranları Oluşturma

### 6.1: Ana Sayfa (index.tsx)

```tsx
import { View, Text, ScrollView, StyleSheet } from 'react-native';
import { useState } from 'react';

export default function AnaSayfa() {
  // Mevcut konum için sahte veri
  const [mevcutKonum] = useState({
    sehir: 'İstanbul',
    sicaklik: 22,
    durum: 'Güneşli',
    nem: 65,
    ruzgar: 15,
    hissedilen: 24,
  });
  
  // 7 günlük tahmin
  const gunlukTahmin = [
    { gun: 'Pzt', sicaklik: 22, min: 18, durum: '☀️' },
    { gun: 'Sal', sicaklik: 20, min: 16, durum: '⛅' },
    { gun: 'Çar', sicaklik: 18, min: 14, durum: '🌧️' },
    { gun: 'Per', sicaklik: 19, min: 15, durum: '⛅' },
    { gun: 'Cum', sicaklik: 23, min: 17, durum: '☀️' },
    { gun: 'Cmt', sicaklik: 25, min: 19, durum: '☀️' },
    { gun: 'Paz', sicaklik: 24, min: 18, durum: '⛅' },
  ];
  
  return (
    <ScrollView style={styles.container}>
      {/* Ana Hava Durumu Kartı */}
      <View style={styles.anaKart}>
        <Text style={styles.sehirAdi}>{mevcutKonum.sehir}</Text>
        <Text style={styles.tarih}>Bugün</Text>
        
        <Text style={styles.sicaklikBuyuk}>{mevcutKonum.sicaklik}°</Text>
        <Text style={styles.durumMetin}>{mevcutKonum.durum}</Text>
        <Text style={styles.hissedilenMetin}>
          Hissedilen: {mevcutKonum.hissedilen}°
        </Text>
        
        {/* Detaylar */}
        <View style={styles.detaylarKutu}>
          <View style={styles.detayItem}>
            <Text style={styles.detayBaslik}>Nem</Text>
            <Text style={styles.detayDeger}>%{mevcutKonum.nem}</Text>
          </View>
          <View style={styles.detayItem}>
            <Text style={styles.detayBaslik}>Rüzgar</Text>
            <Text style={styles.detayDeger}>{mevcutKonum.ruzgar} km/s</Text>
          </View>
        </View>
      </View>
      
      {/* 7 Günlük Tahmin */}
      <View style={styles.tahminBolum}>
        <Text style={styles.bolumBaslik}>7 Günlük Tahmin</Text>
        
        <ScrollView 
          horizontal={true}
          showsHorizontalScrollIndicator={false}
          style={styles.yatayScroll}
        >
          {gunlukTahmin.map((item, index) => (
            <View key={index} style={styles.gunKutu}>
              <Text style={styles.gunAdi}>{item.gun}</Text>
              <Text style={styles.emoji}>{item.durum}</Text>
              <Text style={styles.maxSicaklik}>{item.sicaklik}°</Text>
              <Text style={styles.minSicaklik}>{item.min}°</Text>
            </View>
          ))}
        </ScrollView>
      </View>
      
      {/* Saatlik Tahmin */}
      <View style={styles.saatlikBolum}>
        <Text style={styles.bolumBaslik}>Saatlik Tahmin</Text>
        
        {[
          { saat: 'Şimdi', sicaklik: 22, durum: '☀️' },
          { saat: '14:00', sicaklik: 24, durum: '☀️' },
          { saat: '15:00', sicaklik: 25, durum: '⛅' },
          { saat: '16:00', sicaklik: 24, durum: '⛅' },
          { saat: '17:00', sicaklik: 22, durum: '🌤️' },
        ].map((item, index) => (
          <View key={index} style={styles.saatKutu}>
            <Text style={styles.saatMetin}>{item.saat}</Text>
            <Text style={styles.saatEmoji}>{item.durum}</Text>
            <Text style={styles.saatSicaklik}>{item.sicaklik}°</Text>
          </View>
        ))}
      </View>
    </ScrollView>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#E3F2FD',
  },
  
  // Ana kart stilleri
  anaKart: {
    backgroundColor: '#FFFFFF',
    margin: 20,
    borderRadius: 25,
    padding: 30,
    alignItems: 'center',
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 4 },
    shadowOpacity: 0.15,
    shadowRadius: 12,
    elevation: 8,
  },
  sehirAdi: {
    fontSize: 32,
    fontWeight: 'bold',
    color: '#1976D2',
  },
  tarih: {
    fontSize: 16,
    color: '#757575',
    marginTop: 5,
    marginBottom: 20,
  },
  sicaklikBuyuk: {
    fontSize: 80,
    fontWeight: '200',
    color: '#FF5722',
    marginVertical: 10,
  },
  durumMetin: {
    fontSize: 24,
    color: '#424242',
    marginBottom: 5,
  },
  hissedilenMetin: {
    fontSize: 16,
    color: '#757575',
    marginBottom: 20,
  },
  
  // Detaylar kutusu
  detaylarKutu: {
    flexDirection: 'row',
    gap: 40,
    borderTopWidth: 1,
    borderTopColor: '#E0E0E0',
    paddingTop: 20,
    width: '100%',
    justifyContent: 'center',
  },
  detayItem: {
    alignItems: 'center',
  },
  detayBaslik: {
    fontSize: 14,
    color: '#757575',
    marginBottom: 5,
  },
  detayDeger: {
    fontSize: 20,
    fontWeight: 'bold',
    color: '#424242',
  },
  
  // 7 günlük tahmin
  tahminBolum: {
    backgroundColor: '#FFFFFF',
    marginHorizontal: 20,
    marginBottom: 20,
    borderRadius: 20,
    padding: 20,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.1,
    shadowRadius: 8,
    elevation: 4,
  },
  bolumBaslik: {
    fontSize: 20,
    fontWeight: 'bold',
    color: '#1976D2',
    marginBottom: 15,
  },
  yatayScroll: {
    flexDirection: 'row',
  },
  gunKutu: {
    backgroundColor: '#F5F5F5',
    borderRadius: 15,
    padding: 15,
    marginRight: 12,
    alignItems: 'center',
    width: 70,
  },
  gunAdi: {
    fontSize: 14,
    color: '#424242',
    fontWeight: '600',
    marginBottom: 8,
  },
  emoji: {
    fontSize: 28,
    marginVertical: 5,
  },
  maxSicaklik: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#FF5722',
    marginTop: 5,
  },
  minSicaklik: {
    fontSize: 14,
    color: '#757575',
    marginTop: 2,
  },
  
  // Saatlik tahmin
  saatlikBolum: {
    backgroundColor: '#FFFFFF',
    marginHorizontal: 20,
    marginBottom: 20,
    borderRadius: 20,
    padding: 20,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.1,
    shadowRadius: 8,
    elevation: 4,
  },
  saatKutu: {
    flexDirection: 'row',
    alignItems: 'center',
    paddingVertical: 12,
    borderBottomWidth: 1,
    borderBottomColor: '#F5F5F5',
  },
  saatMetin: {
    fontSize: 16,
    color: '#424242',
    width: 80,
  },
  saatEmoji: {
    fontSize: 24,
    marginHorizontal: 20,
  },
  saatSicaklik: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#FF5722',
    marginLeft: 'auto',
  },
});
```

### 6.2: Arama Ekranı (arama.tsx)

```tsx
import { View, Text, TextInput, ScrollView, StyleSheet, Pressable } from 'react-native';
import { useState } from 'react';

export default function AramaEkrani() {
  const [aramaMetni, setAramaMetni] = useState('');
  const [sonuclar, setSonuclar] = useState<any[]>([]);
  
  // Sahte şehir veritabanı
  const sehirler = [
    { id: 1, sehir: 'İstanbul', sicaklik: 22, durum: 'Güneşli', ulke: 'Türkiye' },
    { id: 2, sehir: 'Ankara', sicaklik: 18, durum: 'Bulutlu', ulke: 'Türkiye' },
    { id: 3, sehir: 'İzmir', sicaklik: 26, durum: 'Güneşli', ulke: 'Türkiye' },
    { id: 4, sehir: 'Antalya', sicaklik: 28, durum: 'Açık', ulke: 'Türkiye' },
    { id: 5, sehir: 'Bursa', sicaklik: 20, durum: 'Yağmurlu', ulke: 'Türkiye' },
    { id: 6, sehir: 'Adana', sicaklik: 30, durum: 'Sıcak', ulke: 'Türkiye' },
    { id: 7, sehir: 'Trabzon', sicaklik: 19, durum: 'Yağmurlu', ulke: 'Türkiye' },
  ];
  
  // Arama fonksiyonu
  const aramaYap = (metin: string) => {
    setAramaMetni(metin);
    
    if (metin.length > 0) {
      // Şehir adında arama metni geçiyorsa filtrele
      const filtrelenmis = sehirler.filter(sehir =>
        sehir.sehir.toLowerCase().includes(metin.toLowerCase())
      );
      setSonuclar(filtrelenmis);
    } else {
      setSonuclar([]);
    }
  };
  
  // Şehir seçildiğinde
  const sehirSec = (sehir: any) => {
    console.log('Seçilen şehir:', sehir.sehir);
    // Burada normalde detay ekranına yönlendirilir
  };
  
  return (
    <View style={styles.container}>
      {/* Arama Kutusu */}
      <View style={styles.aramaKutusu}>
        <Text style={styles.aramaIcon}>🔍</Text>
        <TextInput
          style={styles.aramaInput}
          placeholder="Şehir ara..."
          value={aramaMetni}
          onChangeText={aramaYap}
          placeholderTextColor="#999999"
        />
        {aramaMetni.length > 0 && (
          <Pressable onPress={() => aramaYap('')}>
            <Text style={styles.temizleButon}>✕</Text>
          </Pressable>
        )}
      </View>
      
      {/* Sonuçlar */}
      <ScrollView style={styles.sonuclarContainer}>
        {aramaMetni.length === 0 ? (
          // Arama yapılmadıysa popüler şehirler
          <View>
            <Text style={styles.baslik}>Popüler Şehirler</Text>
            {sehirler.slice(0, 5).map((sehir) => (
              <Pressable
                key={sehir.id}
                style={styles.sehirKart}
                onPress={() => sehirSec(sehir)}
              >
                <View style={styles.sehirBilgi}>
                  <Text style={styles.sehirAdi}>{sehir.sehir}</Text>
                  <Text style={styles.ulke}>{sehir.ulke}</Text>
                </View>
                <View style={styles.havaBilgi}>
                  <Text style={styles.sicaklik}>{sehir.sicaklik}°</Text>
                  <Text style={styles.durum}>{sehir.durum}</Text>
                </View>
              </Pressable>
            ))}
          </View>
        ) : sonuclar.length > 0 ? (
          // Arama sonuçları
          <View>
            <Text style={styles.baslik}>
              {sonuclar.length} sonuç bulundu
            </Text>
            {sonuclar.map((sehir) => (
              <Pressable
                key={sehir.id}
                style={styles.sehirKart}
                onPress={() => sehirSec(sehir)}
              >
                <View style={styles.sehirBilgi}>
                  <Text style={styles.sehirAdi}>{sehir.sehir}</Text>
                  <Text style={styles.ulke}>{sehir.ulke}</Text>
                </View>
                <View style={styles.havaBilgi}>
                  <Text style={styles.sicaklik}>{sehir.sicaklik}°</Text>
                  <Text style={styles.durum}>{sehir.durum}</Text>
                </View>
              </Pressable>
            ))}
          </View>
        ) : (
          // Sonuç bulunamadı
          <View style={styles.sonucYokKutu}>
            <Text style={styles.sonucYokMetin}>
              "{aramaMetni}" için sonuç bulunamadı
            </Text>
          </View>
        )}
      </ScrollView>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#F5F5F5',
  },
  
  // Arama kutusu
  aramaKutusu: {
    flexDirection: 'row',
    alignItems: 'center',
    backgroundColor: '#FFFFFF',
    margin: 20,
    paddingHorizontal: 15,
    borderRadius: 15,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.1,
    shadowRadius: 4,
    elevation: 3,
  },
  aramaIcon: {
    fontSize: 20,
    marginRight: 10,
  },
  aramaInput: {
    flex: 1,
    paddingVertical: 15,
    fontSize: 16,
    color: '#424242',
  },
  temizleButon: {
    fontSize: 20,
    color: '#757575',
    paddingLeft: 10,
  },
  
  // Sonuçlar
  sonuclarContainer: {
    flex: 1,
    paddingHorizontal: 20,
  },
  baslik: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#424242',
    marginBottom: 15,
  },
  
  // Şehir kartı
  sehirKart: {
    backgroundColor: '#FFFFFF',
    borderRadius: 15,
    padding: 20,
    marginBottom: 12,
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 1 },
    shadowOpacity: 0.1,
    shadowRadius: 3,
    elevation: 2,
  },
  sehirBilgi: {
    flex: 1,
  },
  sehirAdi: {
    fontSize: 20,
    fontWeight: 'bold',
    color: '#1976D2',
    marginBottom: 4,
  },
  ulke: {
    fontSize: 14,
    color: '#757575',
  },
  havaBilgi: {
    alignItems: 'flex-end',
  },
  sicaklik: {
    fontSize: 28,
    fontWeight: 'bold',
    color: '#FF5722',
    marginBottom: 4,
  },
  durum: {
    fontSize: 14,
    color: '#424242',
  },
  
  // Sonuç yok
  sonucYokKutu: {
    alignItems: 'center',
    justifyContent: 'center',
    paddingVertical: 60,
  },
  sonucYokMetin: {
    fontSize: 16,
    color: '#757575',
    textAlign: 'center',
  },
});
```

### 6.3: Favoriler Ekranı (favoriler.tsx)

```tsx
import { View, Text, ScrollView, StyleSheet, Pressable } from 'react-native';
import { useState } from 'react';

export default function FavorilerEkrani() {
  // Favori şehirler state'i
  const [favoriler, setFavoriler] = useState([
    { id: 1, sehir: 'İstanbul', sicaklik: 22, durum: 'Güneşli', nem: 65 },
    { id: 3, sehir: 'İzmir', sicaklik: 26, durum: 'Güneşli', nem: 58 },
    { id: 4, sehir: 'Antalya', sicaklik: 28, durum: 'Açık', nem: 62 },
  ]);
  
  // Favoriden çıkar
  const favoridenCikar = (id: number) => {
    setFavoriler(favoriler.filter(fav => fav.id !== id));
  };
  
  return (
    <View style={styles.container}>
      {favoriler.length > 0 ? (
        <ScrollView style={styles.scrollContainer}>
          <Text style={styles.baslik}>
            {favoriler.length} Favori Şehir
          </Text>
          
          {favoriler.map((sehir) => (
            <View key={sehir.id} style={styles.favoriKart}>
              {/* Sol taraf - Şehir bilgisi */}
              <View style={styles.kartSol}>
                <Text style={styles.sehirAdi}>{sehir.sehir}</Text>
                <Text style={styles.durum}>{sehir.durum}</Text>
                
                <View style={styles.detayKutu}>
                  <Text style={styles.detayMetin}>Nem: %{sehir.nem}</Text>
                </View>
              </View>
              
              {/* Sağ taraf - Sıcaklık ve buton */}
              <View style={styles.kartSag}>
                <Text style={styles.sicaklik}>{sehir.sicaklik}°</Text>
                
                <Pressable
                  style={styles.cikarButon}
                  onPress={() => favoridenCikar(sehir.id)}
                >
                  <Text style={styles.cikarButonMetin}>Çıkar</Text>
                </Pressable>
              </View>
            </View>
          ))}
          
          {/* Bilgi notu */}
          <View style={styles.bilgiNotu}>
            <Text style={styles.bilgiMetin}>
              💡 Favori şehirleriniz burada görünür. Arama ekranından yeni şehirler ekleyebilirsiniz.
            </Text>
          </View>
        </ScrollView>
      ) : (
        // Favori yoksa
        <View style={styles.bosKutu}>
          <Text style={styles.bosEmoji}>⭐</Text>
          <Text style={styles.bosBaslik}>Henüz favori yok</Text>
          <Text style={styles.bosMetin}>
            Arama ekranından şehir arayıp favorilerinize ekleyebilirsiniz.
          </Text>
        </View>
      )}
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#F5F5F5',
  },
  scrollContainer: {
    flex: 1,
    padding: 20,
  },
  baslik: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#1976D2',
    marginBottom: 20,
  },
  
  // Favori kartı
  favoriKart: {
    backgroundColor: '#FFFFFF',
    borderRadius: 20,
    padding: 20,
    marginBottom: 15,
    flexDirection: 'row',
    justifyContent: 'space-between',
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.1,
    shadowRadius: 8,
    elevation: 4,
  },
  kartSol: {
    flex: 1,
  },
  sehirAdi: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#424242',
    marginBottom: 5,
  },
  durum: {
    fontSize: 16,
    color: '#757575',
    marginBottom: 10,
  },
  detayKutu: {
    backgroundColor: '#E3F2FD',
    paddingHorizontal: 12,
    paddingVertical: 6,
    borderRadius: 8,
    alignSelf: 'flex-start',
  },
  detayMetin: {
    fontSize: 14,
    color: '#1976D2',
    fontWeight: '600',
  },
  
  // Sağ taraf
  kartSag: {
    alignItems: 'flex-end',
    justifyContent: 'space-between',
  },
  sicaklik: {
    fontSize: 48,
    fontWeight: 'bold',
    color: '#FF5722',
  },
  cikarButon: {
    backgroundColor: '#F44336',
    paddingHorizontal: 16,
    paddingVertical: 8,
    borderRadius: 10,
  },
  cikarButonMetin: {
    color: '#FFFFFF',
    fontSize: 14,
    fontWeight: 'bold',
  },
  
  // Bilgi notu
  bilgiNotu: {
    backgroundColor: '#FFF9C4',
    borderRadius: 15,
    padding: 20,
    marginTop: 20,
    marginBottom: 20,
  },
  bilgiMetin: {
    fontSize: 14,
    color: '#424242',
    lineHeight: 20,
  },
  
  // Boş durum
  bosKutu: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    padding: 40,
  },
  bosEmoji: {
    fontSize: 80,
    marginBottom: 20,
  },
  bosBaslik: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#424242',
    marginBottom: 10,
    textAlign: 'center',
  },
  bosMetin: {
    fontSize: 16,
    color: '#757575',
    textAlign: 'center',
    lineHeight: 24,
  },
});
```

---

## Bölüm 7: Projeyi Çalıştırma ve Test

### Adım 7.1: Projeyi Başlatma

Terminal'de proje klasöründe:

```bash
npx expo start
```

### Adım 7.2: Test Etme

1. **Telefonda Test:** Expo Go uygulamasıyla QR kodu okutun
2. **Emülatörde Test:** Android Studio veya iOS Simulator kullanın
3. **Web'de Test:** `w` tuşuna basarak tarayıcıda açın

### Adım 7.3: Kontrol Listesi

✅ Ana sayfa açılıyor mu?
✅ Tab'lar arasında geçiş çalışıyor mu?
✅ Arama kutusuna yazınca filtreleme yapıyor mu?
✅ ScrollView ile kaydırma çalışıyor mu?
✅ Favorilerden çıkarma butonu çalışıyor mu?
✅ Stiller doğru görünüyor mu?

---

## Bölüm 8: Öğrendikleriniz - Özet

### 🎨 StyleSheet
- `StyleSheet.create()` ile stil tanımlama
- Birden fazla stil birleştirme: `style={[stil1, stil2]}`
- Koşullu stil uygulama
- Flex layout ile düzenleme
- Shadow ve elevation ile gölgeler

### 🔄 useState
- `const [deger, setDeger] = useState(baslangic)` sözdizimi
- Metin girişi için state kullanımı
- Obje olarak state tutma
- State güncellemesi ile component yeniden render

### 📜 ScrollView
- Dikey kaydırma (varsayılan)
- Yatay kaydırma: `horizontal={true}`
- Kaydırma çubuğunu gizleme
- İçerik uzun olduğunda otomatik kaydırma

### 🧭 BottomTabNavigator
- Expo Router ile tab yapısı
- Tab bar stillendirme
- Icon ve başlık ekleme
- Ekranlar arası geçiş

---

## Bölüm 9: Alıştırmalar

### Alıştırma 1: Yeni Stil Ekle
Ana sayfadaki sıcaklık değerini 30°'den yüksekse kırmızı, 20-30° arasıysa turuncu, 20°'den düşükse mavi gösterin.

### Alıştırma 2: Yeni State Ekle
Arama ekranına "Sadece güneşli havalar" adında bir checkbox ekleyin ve işaretlendiğinde sadece güneşli şehirleri gösterin.

### Alıştırma 3: ScrollView Geliştir
Ana sayfaya saatlik tahmin yerine "Son 24 saat" adında yatay kaydırmalı bir bölüm ekleyin.

### Alıştırma 4: Yeni Tab Ekle
"Ayarlar" adında yeni bir tab ekleyin. Bu ekranda:
- Sıcaklık birimi seçimi (°C / °F)
- Koyu tema açma/kapama
- Bildirim tercihleri

---

## Sonuç

Tebrikler! 🎉 Bu derste şunları öğrendiniz:

1. ✅ **StyleSheet** ile profesyonel stil yönetimi
2. ✅ **useState** ile kullanıcı etkileşimlerini yönetme
3. ✅ **ScrollView** ile uzun içerikleri kaydırılabilir hale getirme
4. ✅ **BottomTabNavigator** ile çok ekranlı uygulama yapısı
5. ✅ Gerçek dünya uygulaması: Hava Durumu Uygulaması

### Bir Sonraki Adımlar

- 🔥 Gerçek API entegrasyonu (OpenWeatherMap)
- 💾 AsyncStorage ile verileri kalıcı hale getirme
- 🔔 Push bildirimleri ekleme
- 🌙 Karanlık tema desteği
- 🗺️ Harita entegrasyonu

Başarılar! 🚀
