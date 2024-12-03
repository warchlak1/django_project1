from folder_aplikacji.models import Osoba, Stanowisko
from folder_aplikacji.serializers import OsobaSerializer, StanowiskoSerializer
from rest_framework.renderers import JSONRenderer
from rest_framework.parsers import JSONParser

# nowe obiekty

stanowisko = stanowisko.objects.create(nazwa = "Kierownik", opis = "Zarządza")
osoba = Osoba.objects.create(imie = "Jan", nazwisko = "Malinowski", plec = 2, stanowisko = stanowisko)

# inicjalizacja serializer
osoba_serializer = OsobaSerializer(osoba)
print(osoba_serializer.data)

# ('id': 6, "imie": Jan, "nazwisko" : Malinowski, "plec": Mężczyzna, "stanowisko.id": 4, 'data_dodania': osoba.data_dodania)

osoba_json = JSONRenderer().render(osoba_serializer.data)

# ('id': 6, "imie": Jan, "nazwisko" : Malinowski, "plec": Mężczyzna, "stanowisko": 4, 'data_dodania': 2024-12-03)

import io
# strumien danych json
stream = io.BytesIO(osoba_json)

data = JSONParser().parse(stream)

# obiekt deserializer JSON
deserializer = OsobaSerializer(data = data)

# walidacja danych
print(deserializer.is_valid())
print(deserializer.errors)

# true

# błędne dane
invalid_data = {'imię': 'Adam', 'nazwisko': 'Nowak', 'płeć': 'Nieznany', stanowisko: stanowisko.id}
invalid_serializer = OsobaSerializer(data = invalid_data)
print(invalid_serializer.is_valid())
print(invalid_serializer.errors)

# False
# {'płeć': ['"Nieznany" is not a valid choice.']}

# zapis do BD
if deserializer.is_valid():
    deserializer.save()

# serializacja do JSON 
stanowisko_json = JSONRenderer().render(stanowisko_serializer.data) 
print(stanowisko_json)

# inicjowanie serializera dla Stanowiska
stanowisko_serializer = StanowiskoSerializer(stanowisko)
print(stanowisko_serializer.data)

# deserializacja z JSON 
stream = io.BytesIO(stanowisko_json) 
data = JSONParser().parse(stream) 

deserializer = StanowiskoSerializer(data = data) 
print(deserializer.is_valid()) 

if deserializer.is_valid():
   deseriaizer.save