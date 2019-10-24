
### Weather API Challenge


```python
%matplotlib
```

    Using matplotlib backend: Qt5Agg
    


```python
# Dependencies and Setup
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
import requests
import time
import json

# Import API key
from api_keys import api_key

# Incorporated citipy to determine city based on latitude and longitude
from citipy import citipy

# Output File (CSV)
output_data_file = "output_data/cities.csv"

# Range of latitudes and longitudes
lat_range = (-90, 90)
lng_range = (-180, 180)
```


```python
# List for holding lat_lngs and cities
lat_lngs = []
cities = []

# Create a set of random lat and lng combinations
lats = np.random.uniform(low=-90.000, high=90.000, size=1500)
lngs = np.random.uniform(low=-180.000, high=180.000, size=1500)
lat_lngs = zip(lats, lngs)

# Identify nearest city for each lat, lng combination
for lat_lng in lat_lngs:
    city = citipy.nearest_city(lat_lng[0], lat_lng[1]).city_name
    
    # If the city is unique, then add it to a our cities list
    if city not in cities:
        cities.append(city)

# Print the city count to confirm sufficient count
len(cities)
```




    615




```python
# print(cities)
```


```python
# Set up query url with API key
url = "http://api.openweathermap.org/data/2.5/forecast?"
units = "Imperial"

query_url = f'{url}appid={api_key}&units={units}'

# response = requests.get(f"{query_url}&q={city}").json()

# Create list to append data later
weather_data = []
```


```python
# # Test data retrieval with one city. Find:
#     name
#     countries
#     latitude
#     longitude
#     humidity
#     temperature
#     wind
#     clouds
    
# Test response
response1 = requests.get(f'{query_url}&q=Esperance').json()

# Confirm Keys
response1.keys()

# Print main keys to use
temp = response1['list']
city_list = response1['city']

# Call test variables
test_name = response1['city']['name']
test_country = response1['city']['country']
test_lat = response1['city']['coord']['lat']
test_lng = response1['city']['coord']['lon']
test_temp = temp[0]['main']['temp_max']
test_humidity = temp[0]['main']['humidity']
test_wind = temp[0]['wind']['speed']
test_clouds = temp[0]['clouds']['all']

# Run test
print(test_name)
print(test_country)
print(test_lat)
print(test_lng)
print(test_temp)
print(test_humidity)
print(test_wind)
print(test_clouds)
```

    Esperance
    AU
    -33.8583
    121.8932
    59.9
    69
    27.96
    11
    


```python
#API Call
print(f'Beginning Data Retrieval \n-----------------------------')
records = 0

for city in cities:
    
    try:

        response = requests.get(f"{query_url}&q={city}").json()
        list_var = response['list']
        
        city_name = response['city']['name']
        country_name = response['city']['country']
        lat = response['city']['coord']['lat']
        lng = response['city']['coord']['lon']
        max_temp = list_var[0]['main']['temp_max']
        humidity = list_var[0]['main']['humidity']
        wind = list_var[0]['wind']['speed']
        clouds = list_var[0]['clouds']['all']
        
        weather_data.append({
            "City": city_name,
            "Country": country_name,
            "Lat": lat,
            "Lng": lng,
            "Cloudiness": clouds,
            "Humidity": humidity,
            "Max Temp": max_temp,
            "Wind Speed": wind})
        
        print(f'Processing Record {records} | {city_name}')
        
        records = records + 1
        
        #Prevents API getting blocked
        time.sleep(1.01)
                
    except:
        print("City not found. Skipping...")
    continue
    
print('-----------------------------')
print('Data Retrieval Complete')
print('-----------------------------')
```

    Beginning Data Retrieval 
    -----------------------------
    Processing Record 0 | Luderitz
    Processing Record 1 | Hobart
    Processing Record 2 | Ushuaia
    Processing Record 3 | Vardo
    Processing Record 4 | Longjiang
    Processing Record 5 | Bambous Virieux
    Processing Record 6 | Albany
    Processing Record 7 | Hithadhoo
    Processing Record 8 | Cockburn Town
    Processing Record 9 | Pisco
    City not found. Skipping...
    Processing Record 10 | Tiksi
    Processing Record 11 | Lavrentiya
    Processing Record 12 | Mossoro
    Processing Record 13 | Chuy
    Processing Record 14 | Upernavik
    Processing Record 15 | Rikitea
    Processing Record 16 | Saskylakh
    Processing Record 17 | Dingle
    Processing Record 18 | Busselton
    Processing Record 19 | New Norfolk
    Processing Record 20 | North Bend
    Processing Record 21 | Georgetown
    Processing Record 22 | Kushiro
    Processing Record 23 | Chifeng
    Processing Record 24 | Saint George
    Processing Record 25 | Nome
    Processing Record 26 | Moratuwa
    Processing Record 27 | Thompson
    Processing Record 28 | Butaritari
    Processing Record 29 | Mar del Plata
    Processing Record 30 | Bluff
    Processing Record 31 | Khatanga
    Processing Record 32 | Sao Filipe
    Processing Record 33 | Charters Towers
    Processing Record 34 | Goundam
    Processing Record 35 | Torbay
    Processing Record 36 | Constitucion
    Processing Record 37 | Yellowknife
    Processing Record 38 | Cape Town
    Processing Record 39 | Gulfport
    Processing Record 40 | Ribas do Rio Pardo
    Processing Record 41 | Parakou
    Processing Record 42 | Coahuayana
    Processing Record 43 | Jamestown
    City not found. Skipping...
    Processing Record 44 | Peniche
    Processing Record 45 | Ponta do Sol
    Processing Record 46 | Castro
    Processing Record 47 | Pokrovsk
    Processing Record 48 | Vila
    Processing Record 49 | Trairi
    Processing Record 50 | San Antonio de Cortes
    Processing Record 51 | Clyde River
    Processing Record 52 | Provideniya
    Processing Record 53 | Rovaniemi
    Processing Record 54 | Great Yarmouth
    Processing Record 55 | Kharp
    Processing Record 56 | Mtambile
    Processing Record 57 | Port Alfred
    Processing Record 58 | Zhezkazgan
    Processing Record 59 | Tuktoyaktuk
    Processing Record 60 | Vaini
    Processing Record 61 | Houston
    Processing Record 62 | Hilo
    Processing Record 63 | Punta Arenas
    Processing Record 64 | Argayash
    Processing Record 65 | Susanville
    Processing Record 66 | Tamano
    Processing Record 67 | Banda Aceh
    Processing Record 68 | Mataura
    Processing Record 69 | Shizukuishi
    City not found. Skipping...
    Processing Record 70 | Nichinan
    Processing Record 71 | Kapaa
    Processing Record 72 | Iralaya
    Processing Record 73 | Saint-Philippe
    Processing Record 74 | Victoria
    City not found. Skipping...
    Processing Record 75 | Cardston
    Processing Record 76 | Pitimbu
    Processing Record 77 | Kodiak
    Processing Record 78 | Mahibadhoo
    City not found. Skipping...
    Processing Record 79 | Noumea
    Processing Record 80 | Bubaque
    Processing Record 81 | Ponta Delgada
    Processing Record 82 | Lorengau
    Processing Record 83 | Otjiwarongo
    Processing Record 84 | Mayo
    Processing Record 85 | Eyl
    City not found. Skipping...
    Processing Record 86 | Necochea
    Processing Record 87 | Bandarbeyla
    City not found. Skipping...
    Processing Record 88 | Nalut
    Processing Record 89 | Delta del Tigre
    Processing Record 90 | Kattivakkam
    Processing Record 91 | Barrow
    Processing Record 92 | Chokurdakh
    Processing Record 93 | Taltal
    Processing Record 94 | Cherskiy
    Processing Record 95 | Huarmey
    Processing Record 96 | Pio XII
    Processing Record 97 | Nikolskoye
    Processing Record 98 | La Ronge
    Processing Record 99 | Mileanca
    Processing Record 100 | Yinchuan
    Processing Record 101 | Agadir
    Processing Record 102 | Avarua
    City not found. Skipping...
    Processing Record 103 | Lady Frere
    Processing Record 104 | Meulaboh
    Processing Record 105 | Mana
    Processing Record 106 | Windhoek
    Processing Record 107 | Mezen
    Processing Record 108 | Avera
    City not found. Skipping...
    Processing Record 109 | Cabo San Lucas
    Processing Record 110 | Walvis Bay
    Processing Record 111 | San Quintin
    Processing Record 112 | High Level
    Processing Record 113 | Hermanus
    Processing Record 114 | Lazarev
    Processing Record 115 | Saint Anthony
    Processing Record 116 | Dikson
    Processing Record 117 | Camopi
    Processing Record 118 | Airai
    Processing Record 119 | Grindavik
    City not found. Skipping...
    Processing Record 120 | Amfissa
    City not found. Skipping...
    Processing Record 121 | Codrington
    City not found. Skipping...
    Processing Record 122 | Bredasdorp
    Processing Record 123 | Benghazi
    Processing Record 124 | Sioux Lookout
    Processing Record 125 | Bathsheba
    City not found. Skipping...
    Processing Record 126 | San-Pedro
    Processing Record 127 | Xichang
    Processing Record 128 | Pevek
    Processing Record 129 | Yangjiang
    Processing Record 130 | Severo-Kurilsk
    Processing Record 131 | Benguela
    Processing Record 132 | Ilulissat
    Processing Record 133 | Axim
    Processing Record 134 | Hearst
    Processing Record 135 | Matara
    Processing Record 136 | Maceio
    Processing Record 137 | Altstatten
    Processing Record 138 | Bayangol
    Processing Record 139 | Atuona
    Processing Record 140 | Tarakan
    Processing Record 141 | Bouafle
    Processing Record 142 | Katherine
    Processing Record 143 | Kingisepp
    Processing Record 144 | Araouane
    Processing Record 145 | Hammerfest
    Processing Record 146 | Te Anau
    Processing Record 147 | Hamilton
    Processing Record 148 | Woodward
    City not found. Skipping...
    Processing Record 149 | Carutapera
    City not found. Skipping...
    Processing Record 150 | Kruisfontein
    City not found. Skipping...
    Processing Record 151 | Vostok
    Processing Record 152 | Beringovskiy
    Processing Record 153 | Ballitoville
    Processing Record 154 | Laguna
    Processing Record 155 | Vao
    Processing Record 156 | Scottsbluff
    Processing Record 157 | Sola
    Processing Record 158 | Mogadishu
    Processing Record 159 | Auki
    Processing Record 160 | Mount Gambier
    Processing Record 161 | Port Elizabeth
    City not found. Skipping...
    Processing Record 162 | Arraial do Cabo
    Processing Record 163 | Zyryanka
    Processing Record 164 | Jodhpur
    Processing Record 165 | Churapcha
    Processing Record 166 | Puerto Ayora
    Processing Record 167 | Komsomolskiy
    Processing Record 168 | Chadiza
    Processing Record 169 | Halifax
    Processing Record 170 | Vryburg
    Processing Record 171 | Sembakung
    Processing Record 172 | Sagua de Tanamo
    Processing Record 173 | Havre-Saint-Pierre
    Processing Record 174 | Mahajanga
    Processing Record 175 | Atar
    Processing Record 176 | Coquimbo
    Processing Record 177 | Slany
    City not found. Skipping...
    Processing Record 178 | Nagaur
    Processing Record 179 | Turukhansk
    Processing Record 180 | Montepuez
    Processing Record 181 | Prince Rupert
    Processing Record 182 | Klaksvik
    Processing Record 183 | Ostrovnoy
    Processing Record 184 | Mahebourg
    City not found. Skipping...
    Processing Record 185 | Nizwa
    Processing Record 186 | Mikkeli
    Processing Record 187 | La Grande
    Processing Record 188 | Qaanaaq
    City not found. Skipping...
    City not found. Skipping...
    Processing Record 189 | Haapiti
    Processing Record 190 | Porto Novo
    Processing Record 191 | Portland
    Processing Record 192 | Carballo
    Processing Record 193 | Dickinson
    City not found. Skipping...
    Processing Record 194 | Tasiilaq
    City not found. Skipping...
    City not found. Skipping...
    Processing Record 195 | Vila Franca do Campo
    City not found. Skipping...
    Processing Record 196 | College
    Processing Record 197 | Hokitika
    Processing Record 198 | Shitanjing
    Processing Record 199 | Bereda
    Processing Record 200 | Teberda
    Processing Record 201 | Deputatskiy
    Processing Record 202 | Jalu
    Processing Record 203 | Tequila
    Processing Record 204 | Hargeysa
    Processing Record 205 | Vieques
    Processing Record 206 | Rio Gallegos
    Processing Record 207 | Svetlogorsk
    Processing Record 208 | East London
    Processing Record 209 | Luganville
    Processing Record 210 | Aklavik
    City not found. Skipping...
    Processing Record 211 | Hambantota
    Processing Record 212 | Thenzawl
    Processing Record 213 | Bethel
    Processing Record 214 | Yumen
    Processing Record 215 | Guerrero Negro
    Processing Record 216 | Inhambane
    Processing Record 217 | Port Blair
    Processing Record 218 | Hervey Bay
    City not found. Skipping...
    Processing Record 219 | Mehamn
    Processing Record 220 | Mayumba
    Processing Record 221 | Hasaki
    Processing Record 222 | Petropavlovsk-Kamchatskiy
    Processing Record 223 | Kibakwe
    Processing Record 224 | Kalianget
    Processing Record 225 | Srednekolymsk
    Processing Record 226 | Kavieng
    Processing Record 227 | Sovetskiy
    Processing Record 228 | Iqaluit
    Processing Record 229 | Nioro
    City not found. Skipping...
    Processing Record 230 | Beruwala
    Processing Record 231 | Broome
    Processing Record 232 | Nishihara
    Processing Record 233 | Raudeberg
    Processing Record 234 | Baruun-Urt
    Processing Record 235 | Saquena
    Processing Record 236 | Sechura
    Processing Record 237 | Cumberland
    Processing Record 238 | Shahr-e Babak
    Processing Record 239 | Darnah
    Processing Record 240 | Hami
    Processing Record 241 | Demirci
    Processing Record 242 | Nova Vicosa
    Processing Record 243 | Kota Kinabalu
    Processing Record 244 | Faanui
    Processing Record 245 | Pokhara
    Processing Record 246 | Panzhihua
    Processing Record 247 | Iskilip
    Processing Record 248 | Le Port
    Processing Record 249 | Kingsland
    Processing Record 250 | Longyearbyen
    Processing Record 251 | Olinda
    Processing Record 252 | Henties Bay
    City not found. Skipping...
    Processing Record 253 | Elizabeth City
    Processing Record 254 | Pechenga
    Processing Record 255 | Meiganga
    Processing Record 256 | Emerald
    Processing Record 257 | Saldanha
    Processing Record 258 | Balabac
    Processing Record 259 | Saint-Augustin
    Processing Record 260 | Lufilufi
    Processing Record 261 | Narsaq
    Processing Record 262 | Walla Walla
    Processing Record 263 | Fairbanks
    Processing Record 264 | Chumikan
    Processing Record 265 | Bad Doberan
    Processing Record 266 | Jiuquan
    Processing Record 267 | Northam
    Processing Record 268 | Davila
    Processing Record 269 | Jumla
    Processing Record 270 | Thetford Mines
    Processing Record 271 | Mahanoro
    Processing Record 272 | Beloha
    Processing Record 273 | Katsuura
    City not found. Skipping...
    Processing Record 274 | Faya
    Processing Record 275 | Lata
    City not found. Skipping...
    Processing Record 276 | Ahipara
    Processing Record 277 | Norman Wells
    City not found. Skipping...
    Processing Record 278 | Salisbury
    Processing Record 279 | The Valley
    Processing Record 280 | Launceston
    Processing Record 281 | Paamiut
    Processing Record 282 | Tabou
    Processing Record 283 | Carnarvon
    Processing Record 284 | Margate
    Processing Record 285 | Athabasca
    Processing Record 286 | Roseburg
    Processing Record 287 | Lakhnadon
    Processing Record 288 | Vertientes
    City not found. Skipping...
    Processing Record 289 | Marawi
    Processing Record 290 | Belyy Yar
    Processing Record 291 | Nova Londrina
    Processing Record 292 | Cabedelo
    Processing Record 293 | Russkaya Polyana
    Processing Record 294 | Berezovyy
    Processing Record 295 | Abu Kamal
    Processing Record 296 | Esperance
    Processing Record 297 | Pringsewu
    Processing Record 298 | Novaya Igirma
    Processing Record 299 | Talnakh
    Processing Record 300 | Bilma
    Processing Record 301 | Souillac
    Processing Record 302 | Terrace Bay
    Processing Record 303 | Kayes
    Processing Record 304 | Vanimo
    Processing Record 305 | Xifeng
    Processing Record 306 | La Palma
    Processing Record 307 | Pasvalys
    Processing Record 308 | Sisophon
    Processing Record 309 | Tahoua
    Processing Record 310 | Umabay
    Processing Record 311 | Leh
    Processing Record 312 | Berwick
    Processing Record 313 | Quatro Barras
    Processing Record 314 | Rorvik
    Processing Record 315 | Soyo
    Processing Record 316 | Tomatlan
    Processing Record 317 | Melilla
    Processing Record 318 | Vila Velha
    Processing Record 319 | San Patricio
    City not found. Skipping...
    Processing Record 320 | Rio Cauto
    Processing Record 321 | Bowen
    Processing Record 322 | Castanos
    Processing Record 323 | Ngunguru
    Processing Record 324 | Liuhe
    Processing Record 325 | Repnoye
    Processing Record 326 | Edeia
    Processing Record 327 | Marsh Harbour
    City not found. Skipping...
    Processing Record 328 | Weinan
    Processing Record 329 | Fortuna
    Processing Record 330 | San Cristobal
    Processing Record 331 | Kaitangata
    Processing Record 332 | Labuhan
    Processing Record 333 | Palmer
    City not found. Skipping...
    Processing Record 334 | Obihiro
    Processing Record 335 | Ewa Beach
    Processing Record 336 | Basco
    Processing Record 337 | Dunedin
    Processing Record 338 | Nouadhibou
    Processing Record 339 | Shingu
    City not found. Skipping...
    Processing Record 340 | Semey
    Processing Record 341 | Moose Factory
    Processing Record 342 | Ribeira Grande
    City not found. Skipping...
    City not found. Skipping...
    Processing Record 343 | Ambilobe
    Processing Record 344 | Kaitong
    City not found. Skipping...
    Processing Record 345 | Trenggalek
    Processing Record 346 | Honningsvag
    Processing Record 347 | Kobryn
    Processing Record 348 | Pangnirtung
    Processing Record 349 | Leningradskiy
    Processing Record 350 | Fort Beaufort
    Processing Record 351 | Shasta Lake
    Processing Record 352 | Cody
    Processing Record 353 | Great Falls
    Processing Record 354 | Paidha
    Processing Record 355 | Lebu
    Processing Record 356 | Chaiya
    Processing Record 357 | Jimma
    Processing Record 358 | Nizhniy Kuranakh
    Processing Record 359 | Zhigansk
    Processing Record 360 | Masallatah
    Processing Record 361 | Yasnogorsk
    Processing Record 362 | Palma
    Processing Record 363 | Yulara
    Processing Record 364 | Gillette
    Processing Record 365 | Yining
    Processing Record 366 | Comodoro Rivadavia
    Processing Record 367 | Namatanai
    Processing Record 368 | Sorland
    Processing Record 369 | Kuytun
    Processing Record 370 | Palana
    Processing Record 371 | Tsabong
    Processing Record 372 | Kutum
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    Processing Record 373 | Bilibino
    City not found. Skipping...
    Processing Record 374 | Biak
    Processing Record 375 | Tongren
    Processing Record 376 | Bontang
    Processing Record 377 | Husavik
    Processing Record 378 | Along
    Processing Record 379 | Mendi
    Processing Record 380 | Tura
    Processing Record 381 | Muros
    Processing Record 382 | Corpus Christi
    Processing Record 383 | Itarema
    Processing Record 384 | Samana
    Processing Record 385 | Burdur
    Processing Record 386 | Lagoa
    Processing Record 387 | Lindsay
    Processing Record 388 | Maraa
    Processing Record 389 | Nanortalik
    Processing Record 390 | Ailigandi
    Processing Record 391 | Sfantu Gheorghe
    Processing Record 392 | Russellville
    Processing Record 393 | Antalaha
    Processing Record 394 | Kon Tum
    Processing Record 395 | Bang Saphan
    Processing Record 396 | Aswan
    Processing Record 397 | Aanekoski
    Processing Record 398 | Mihaileni
    Processing Record 399 | Kungurtug
    City not found. Skipping...
    Processing Record 400 | Ornskoldsvik
    Processing Record 401 | Pouebo
    Processing Record 402 | Pecos
    Processing Record 403 | Posevnaya
    City not found. Skipping...
    Processing Record 404 | Ambon
    Processing Record 405 | Genhe
    City not found. Skipping...
    Processing Record 406 | Okhotsk
    Processing Record 407 | Gavle
    Processing Record 408 | Kurmanayevka
    Processing Record 409 | Waipawa
    City not found. Skipping...
    Processing Record 410 | Ternate
    Processing Record 411 | Dingzhou
    Processing Record 412 | Jacareacanga
    Processing Record 413 | Camacha
    Processing Record 414 | Isangel
    City not found. Skipping...
    Processing Record 415 | Colares
    Processing Record 416 | Luau
    Processing Record 417 | Tripoli
    Processing Record 418 | Alice Springs
    Processing Record 419 | Anamur
    Processing Record 420 | Parian Dakula
    Processing Record 421 | Bara
    City not found. Skipping...
    City not found. Skipping...
    Processing Record 422 | Teguise
    Processing Record 423 | Cayenne
    Processing Record 424 | Galle
    Processing Record 425 | Port-Cartier
    Processing Record 426 | San Pedro
    Processing Record 427 | Pomabamba
    Processing Record 428 | Kjollefjord
    Processing Record 429 | Slave Lake
    Processing Record 430 | Poum
    Processing Record 431 | Caravelas
    Processing Record 432 | Angoram
    Processing Record 433 | Tarabuco
    Processing Record 434 | Boundiali
    Processing Record 435 | Grand Gaube
    Processing Record 436 | Taywarah
    Processing Record 437 | Puerto Rondon
    Processing Record 438 | Wanning
    Processing Record 439 | Salinopolis
    Processing Record 440 | La Brea
    Processing Record 441 | Kamaishi
    City not found. Skipping...
    Processing Record 442 | Herat
    Processing Record 443 | Kargasok
    Processing Record 444 | Oranjemund
    Processing Record 445 | San Jeronimo
    Processing Record 446 | Nokaneng
    Processing Record 447 | Shakhtinsk
    Processing Record 448 | Port Lincoln
    Processing Record 449 | Aktau
    Processing Record 450 | Hobyo
    Processing Record 451 | Bagdarin
    Processing Record 452 | San Angelo
    Processing Record 453 | Coihaique
    Processing Record 454 | Yatou
    Processing Record 455 | Puri
    Processing Record 456 | Altus
    Processing Record 457 | Neuquen
    Processing Record 458 | Daitari
    City not found. Skipping...
    Processing Record 459 | Grand-Santi
    Processing Record 460 | Ust-Kuyga
    Processing Record 461 | Mairana
    Processing Record 462 | Port Shepstone
    Processing Record 463 | Tulare
    Processing Record 464 | Pacific Grove
    Processing Record 465 | Goderich
    City not found. Skipping...
    Processing Record 466 | Mollendo
    Processing Record 467 | Rapid Valley
    Processing Record 468 | Rome
    City not found. Skipping...
    Processing Record 469 | Lompoc
    Processing Record 470 | Shimoda
    Processing Record 471 | Sobolevo
    Processing Record 472 | Werda
    Processing Record 473 | Atbasar
    Processing Record 474 | Awjilah
    Processing Record 475 | Alta Floresta
    City not found. Skipping...
    City not found. Skipping...
    Processing Record 476 | Salalah
    Processing Record 477 | Ancud
    Processing Record 478 | Traverse City
    City not found. Skipping...
    Processing Record 479 | Am Timan
    Processing Record 480 | Berdigestyakh
    Processing Record 481 | Ouargaye
    Processing Record 482 | Chimore
    Processing Record 483 | Voh
    Processing Record 484 | Itaituba
    Processing Record 485 | Kichera
    Processing Record 486 | Dalnerechensk
    Processing Record 487 | Mangan
    Processing Record 488 | Atambua
    Processing Record 489 | Thunder Bay
    City not found. Skipping...
    Processing Record 490 | Quesnel
    Processing Record 491 | Sur
    City not found. Skipping...
    Processing Record 492 | Bacolod
    Processing Record 493 | Porto Walter
    City not found. Skipping...
    Processing Record 494 | Iracoubo
    Processing Record 495 | Suihua
    Processing Record 496 | Taksimo
    Processing Record 497 | Nogliki
    Processing Record 498 | Surgut
    Processing Record 499 | Cidreira
    Processing Record 500 | Bujumbura
    Processing Record 501 | Amarante do Maranhao
    City not found. Skipping...
    Processing Record 502 | Gushikawa
    Processing Record 503 | Ginda
    Processing Record 504 | Kieta
    Processing Record 505 | Sokoto
    Processing Record 506 | Nelson Bay
    Processing Record 507 | Mugan
    Processing Record 508 | Sayat
    Processing Record 509 | Thanh Hoa
    Processing Record 510 | Tateyama
    Processing Record 511 | Tessalit
    Processing Record 512 | Timiryazevskiy
    Processing Record 513 | Egvekinot
    Processing Record 514 | Champerico
    Processing Record 515 | Thinadhoo
    Processing Record 516 | Hirado
    Processing Record 517 | Vestmannaeyjar
    Processing Record 518 | Dryden
    Processing Record 519 | Padang
    Processing Record 520 | Requena
    Processing Record 521 | Namibe
    Processing Record 522 | Xingcheng
    Processing Record 523 | Opuwo
    Processing Record 524 | Price
    Processing Record 525 | Nakhon Phanom
    Processing Record 526 | Chapais
    City not found. Skipping...
    City not found. Skipping...
    Processing Record 527 | Barreirinha
    Processing Record 528 | Kawalu
    Processing Record 529 | Concepcion del Oro
    Processing Record 530 | Gat
    Processing Record 531 | Richards Bay
    City not found. Skipping...
    Processing Record 532 | Abha
    City not found. Skipping...
    Processing Record 533 | Van
    Processing Record 534 | Cabanas
    Processing Record 535 | Bridlington
    City not found. Skipping...
    Processing Record 536 | Baykit
    Processing Record 537 | Imeni Babushkina
    Processing Record 538 | Muyezerskiy
    Processing Record 539 | Lasa
    Processing Record 540 | Lamu
    Processing Record 541 | Kalmunai
    Processing Record 542 | Santa Maria
    Processing Record 543 | Hovd
    Processing Record 544 | Bom Jardim
    Processing Record 545 | Los Llanos de Aridane
    Processing Record 546 | Najran
    Processing Record 547 | Barguzin
    -----------------------------
    Data Retrieval Complete
    -----------------------------
    


```python
weather_py = pd.DataFrame(weather_data)
weather_py.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City</th>
      <th>Country</th>
      <th>Lat</th>
      <th>Lng</th>
      <th>Cloudiness</th>
      <th>Humidity</th>
      <th>Max Temp</th>
      <th>Wind Speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Luderitz</td>
      <td>NA</td>
      <td>-26.6481</td>
      <td>15.1594</td>
      <td>32</td>
      <td>70</td>
      <td>61.05</td>
      <td>5.01</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Hobart</td>
      <td>AU</td>
      <td>-42.8826</td>
      <td>147.3281</td>
      <td>100</td>
      <td>52</td>
      <td>65.08</td>
      <td>16.96</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Ushuaia</td>
      <td>AR</td>
      <td>-54.8070</td>
      <td>-68.3074</td>
      <td>100</td>
      <td>83</td>
      <td>53.42</td>
      <td>3.42</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Vardo</td>
      <td>US</td>
      <td>39.6165</td>
      <td>-77.7392</td>
      <td>0</td>
      <td>49</td>
      <td>66.31</td>
      <td>5.28</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Longjiang</td>
      <td>CN</td>
      <td>47.3301</td>
      <td>123.1838</td>
      <td>100</td>
      <td>98</td>
      <td>31.78</td>
      <td>13.18</td>
    </tr>
  </tbody>
</table>
</div>




```python
weather_py.count()
```




    City          548
    Country       548
    Lat           548
    Lng           548
    Cloudiness    548
    Humidity      548
    Max Temp      548
    Wind Speed    548
    dtype: int64




```python
#Save to CSV
weather_py.to_csv("weather_data.csv")
```


```python
#Latitude vs. Temperature Plot
plt.scatter(weather_py["Lat"], weather_py["Max Temp"], s=10, marker='o', color='Coral', edgecolor='Black', alpha=0.75)
plt.title(f'City Latitude vs Max Temperature')
plt.xlabel('Latitude')
plt.ylabel('Max Temperature (F)')
plt.grid(True)
plt.savefig("../LatvTemp.png", bbox_inches='tight')
plt.show()
```


![png](output_11_0.png)



```python
#Latitude vs. Humidity Plot
plt.scatter(weather_py["Lat"], weather_py["Humidity"], s=10, marker='o', color='Skyblue', edgecolor='Black', alpha=0.75)
plt.title('City Latitude vs Humidity')
plt.xlabel('Latitude')
plt.ylabel('Humidity (%)')
plt.grid(True)
plt.savefig("../LatvHum.png", bbox_inches='tight')
plt.show()
```


![png](output_12_0.png)



```python
#Latitude vs. Cloudiness Plot
plt.scatter(weather_py["Lat"], weather_py["Cloudiness"], s=10, marker='o', color='red', edgecolor='black', alpha=0.75)
plt.title(f'City Latitude vs Cloudiness')
plt.xlabel('Latitude')
plt.ylabel('Cloudiness (%)')
plt.grid(True)
plt.savefig("../LatvClouds.png", bbox_inches='tight')
plt.show()               
```


![png](output_13_0.png)



```python
# Latitude vs. Wind Speed Plot
plt.scatter(weather_py['Lat'], weather_py['Wind Speed'], s=10, marker='o', color='yellow', edgecolor='black', alpha=0.85)
plt.title(f'City Latitude vs Wind Speed')
plt.xlabel('Latitude')
plt.ylabel('Wind Speed (mph)')
plt.grid(True)
plt.savefig("../LatvWind.png", bbox_inches='tight')
plt.show()
```


![png](output_14_0.png)

