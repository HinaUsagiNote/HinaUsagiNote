---
layout: single
title: 'Boston Housing Dataset 모델'
typora-root-url: ../
categories: Deeplearning_Pytorch.10.Transfer Learning과 Fine Tuning
tag: Pytorch
toc: true
---

# Pretrained Model

- 다른 목적을 위해 미리 학습된 모델.
- Pretrained model을 현재 해결하려는 문제에 이용한다.
- 대부분 내가 만들려는 네트워크 모델에 pretrained model을 포함시켜 사용한다.
    - 이런 방식을 Transfer Learning (전이 학습)이라고 한다.
    - 보통 Feature Extractor block을 재사용한다.

## Pytorch에서 제공하는 Pretrained Model 
- 분야별 라이브러리에서 제공
    - torchvision: https://pytorch.org/vision/stable/models.html
- torch hub 를 이용해 모델과 학습된 parameter를 사용할 수 있다.
    - https://pytorch.org/hub/
- huggingface
    - https://huggingface.co/
- 이외에도 많은 모델과 학습된 paramter가 인터넷상에 공개되 있다.    
    - 딥러닝 모델기반 application을 개발 할 때는 대부분 Transfer Learning을 한다.  
    - 다양한 분야에서 연구된 많은 딥러닝 모델들이 구현되어 공개 되어 있으며 학습된 Parameter들도 제공되고 있다.  
    - [paperswithcode](https://paperswithcode.com/)에서 State Of The Art(SOTA) 논문들과 그 구현된 모델을 확인할 수 있다. 
    
>  **State Of The Art(SOTA)**: 특정 시점에 특정 분야에서 가장 성능이 좋은 모델을 말한다.

## VGGNet Pretrained 모델을 이용해 이미지 분류

- Pytorch가 제공하는 VGG 모델은 ImageNet dataset으로 학습시킨 weight를 제공한다.
    - 120만장의 transet, 1000개의 class로 구성된 데이터셋.
    - Output으로 1000개의 카테고리에 대한 확률을 출력한다. 


```python
# ImageNet 1000개의 class 목록
!pip install wget
import wget
url = 'https://gist.github.com/yrevar/942d3a0ac09ec9e5eb3a/raw/238f720ff059c1f82f368259d1ca4ffa5dd8f9f5/imagenet1000_clsidx_to_labels.txt'
imagenet_filepath = wget.download(url)
```

    Collecting wget
      Downloading wget-3.2.zip (10 kB)
      Preparing metadata (setup.py): started
      Preparing metadata (setup.py): finished with status 'done'
    Building wheels for collected packages: wget
      Building wheel for wget (setup.py): started
      Building wheel for wget (setup.py): finished with status 'done'
      Created wheel for wget: filename=wget-3.2-py3-none-any.whl size=9680 sha256=0e382e4b895d0936f7157b5c819d25f83f71f928a1b151de612c3ce65ebc1b0e
      Stored in directory: c:\users\world\appdata\local\pip\cache\wheels\8b\f1\7f\5c94f0a7a505ca1c81cd1d9208ae2064675d97582078e6c769
    Successfully built wget
    Installing collected packages: wget
    Successfully installed wget-3.2
    100% [..............................................................................] 30564 / 30564


```python
# eval('파이썬코드'): 파이썬 코드를 문자열로 넣으면 알아서 핵석해서 실행
r = eval('1 + 3')
print(r, type(r))
```

    4 <class 'int'>
    


```python
r = eval('[1, 2, 3]')
print(r, type(r))
```

    [1, 2, 3] <class 'list'>
    


```python
r = eval("{0: '사람', 1: '물통'}")
print(r, type(r))
```

    {0: '사람', 1: '물통'} <class 'dict'>
    


```python
with open(imagenet_filepath, 'rt') as fr:
    a = fr.read()
    print(type(a))
    print(a)
```

    <class 'str'>
    {0: 'tench, Tinca tinca',
     1: 'goldfish, Carassius auratus',
     2: 'great white shark, white shark, man-eater, man-eating shark, Carcharodon carcharias',
     3: 'tiger shark, Galeocerdo cuvieri',
     4: 'hammerhead, hammerhead shark',
     5: 'electric ray, crampfish, numbfish, torpedo',
     6: 'stingray',
     7: 'cock',
     8: 'hen',
     9: 'ostrich, Struthio camelus',
     10: 'brambling, Fringilla montifringilla',
     11: 'goldfinch, Carduelis carduelis',
     12: 'house finch, linnet, Carpodacus mexicanus',
     13: 'junco, snowbird',
     14: 'indigo bunting, indigo finch, indigo bird, Passerina cyanea',
     15: 'robin, American robin, Turdus migratorius',
     16: 'bulbul',
     17: 'jay',
     18: 'magpie',
     19: 'chickadee',
     20: 'water ouzel, dipper',
     21: 'kite',
     22: 'bald eagle, American eagle, Haliaeetus leucocephalus',
     23: 'vulture',
     24: 'great grey owl, great gray owl, Strix nebulosa',
     25: 'European fire salamander, Salamandra salamandra',
     26: 'common newt, Triturus vulgaris',
     27: 'eft',
     28: 'spotted salamander, Ambystoma maculatum',
     29: 'axolotl, mud puppy, Ambystoma mexicanum',
     30: 'bullfrog, Rana catesbeiana',
     31: 'tree frog, tree-frog',
     32: 'tailed frog, bell toad, ribbed toad, tailed toad, Ascaphus trui',
     33: 'loggerhead, loggerhead turtle, Caretta caretta',
     34: 'leatherback turtle, leatherback, leathery turtle, Dermochelys coriacea',
     35: 'mud turtle',
     36: 'terrapin',
     37: 'box turtle, box tortoise',
     38: 'banded gecko',
     39: 'common iguana, iguana, Iguana iguana',
     40: 'American chameleon, anole, Anolis carolinensis',
     41: 'whiptail, whiptail lizard',
     42: 'agama',
     43: 'frilled lizard, Chlamydosaurus kingi',
     44: 'alligator lizard',
     45: 'Gila monster, Heloderma suspectum',
     46: 'green lizard, Lacerta viridis',
     47: 'African chameleon, Chamaeleo chamaeleon',
     48: 'Komodo dragon, Komodo lizard, dragon lizard, giant lizard, Varanus komodoensis',
     49: 'African crocodile, Nile crocodile, Crocodylus niloticus',
     50: 'American alligator, Alligator mississipiensis',
     51: 'triceratops',
     52: 'thunder snake, worm snake, Carphophis amoenus',
     53: 'ringneck snake, ring-necked snake, ring snake',
     54: 'hognose snake, puff adder, sand viper',
     55: 'green snake, grass snake',
     56: 'king snake, kingsnake',
     57: 'garter snake, grass snake',
     58: 'water snake',
     59: 'vine snake',
     60: 'night snake, Hypsiglena torquata',
     61: 'boa constrictor, Constrictor constrictor',
     62: 'rock python, rock snake, Python sebae',
     63: 'Indian cobra, Naja naja',
     64: 'green mamba',
     65: 'sea snake',
     66: 'horned viper, cerastes, sand viper, horned asp, Cerastes cornutus',
     67: 'diamondback, diamondback rattlesnake, Crotalus adamanteus',
     68: 'sidewinder, horned rattlesnake, Crotalus cerastes',
     69: 'trilobite',
     70: 'harvestman, daddy longlegs, Phalangium opilio',
     71: 'scorpion',
     72: 'black and gold garden spider, Argiope aurantia',
     73: 'barn spider, Araneus cavaticus',
     74: 'garden spider, Aranea diademata',
     75: 'black widow, Latrodectus mactans',
     76: 'tarantula',
     77: 'wolf spider, hunting spider',
     78: 'tick',
     79: 'centipede',
     80: 'black grouse',
     81: 'ptarmigan',
     82: 'ruffed grouse, partridge, Bonasa umbellus',
     83: 'prairie chicken, prairie grouse, prairie fowl',
     84: 'peacock',
     85: 'quail',
     86: 'partridge',
     87: 'African grey, African gray, Psittacus erithacus',
     88: 'macaw',
     89: 'sulphur-crested cockatoo, Kakatoe galerita, Cacatua galerita',
     90: 'lorikeet',
     91: 'coucal',
     92: 'bee eater',
     93: 'hornbill',
     94: 'hummingbird',
     95: 'jacamar',
     96: 'toucan',
     97: 'drake',
     98: 'red-breasted merganser, Mergus serrator',
     99: 'goose',
     100: 'black swan, Cygnus atratus',
     101: 'tusker',
     102: 'echidna, spiny anteater, anteater',
     103: 'platypus, duckbill, duckbilled platypus, duck-billed platypus, Ornithorhynchus anatinus',
     104: 'wallaby, brush kangaroo',
     105: 'koala, koala bear, kangaroo bear, native bear, Phascolarctos cinereus',
     106: 'wombat',
     107: 'jellyfish',
     108: 'sea anemone, anemone',
     109: 'brain coral',
     110: 'flatworm, platyhelminth',
     111: 'nematode, nematode worm, roundworm',
     112: 'conch',
     113: 'snail',
     114: 'slug',
     115: 'sea slug, nudibranch',
     116: 'chiton, coat-of-mail shell, sea cradle, polyplacophore',
     117: 'chambered nautilus, pearly nautilus, nautilus',
     118: 'Dungeness crab, Cancer magister',
     119: 'rock crab, Cancer irroratus',
     120: 'fiddler crab',
     121: 'king crab, Alaska crab, Alaskan king crab, Alaska king crab, Paralithodes camtschatica',
     122: 'American lobster, Northern lobster, Maine lobster, Homarus americanus',
     123: 'spiny lobster, langouste, rock lobster, crawfish, crayfish, sea crawfish',
     124: 'crayfish, crawfish, crawdad, crawdaddy',
     125: 'hermit crab',
     126: 'isopod',
     127: 'white stork, Ciconia ciconia',
     128: 'black stork, Ciconia nigra',
     129: 'spoonbill',
     130: 'flamingo',
     131: 'little blue heron, Egretta caerulea',
     132: 'American egret, great white heron, Egretta albus',
     133: 'bittern',
     134: 'crane',
     135: 'limpkin, Aramus pictus',
     136: 'European gallinule, Porphyrio porphyrio',
     137: 'American coot, marsh hen, mud hen, water hen, Fulica americana',
     138: 'bustard',
     139: 'ruddy turnstone, Arenaria interpres',
     140: 'red-backed sandpiper, dunlin, Erolia alpina',
     141: 'redshank, Tringa totanus',
     142: 'dowitcher',
     143: 'oystercatcher, oyster catcher',
     144: 'pelican',
     145: 'king penguin, Aptenodytes patagonica',
     146: 'albatross, mollymawk',
     147: 'grey whale, gray whale, devilfish, Eschrichtius gibbosus, Eschrichtius robustus',
     148: 'killer whale, killer, orca, grampus, sea wolf, Orcinus orca',
     149: 'dugong, Dugong dugon',
     150: 'sea lion',
     151: 'Chihuahua',
     152: 'Japanese spaniel',
     153: 'Maltese dog, Maltese terrier, Maltese',
     154: 'Pekinese, Pekingese, Peke',
     155: 'Shih-Tzu',
     156: 'Blenheim spaniel',
     157: 'papillon',
     158: 'toy terrier',
     159: 'Rhodesian ridgeback',
     160: 'Afghan hound, Afghan',
     161: 'basset, basset hound',
     162: 'beagle',
     163: 'bloodhound, sleuthhound',
     164: 'bluetick',
     165: 'black-and-tan coonhound',
     166: 'Walker hound, Walker foxhound',
     167: 'English foxhound',
     168: 'redbone',
     169: 'borzoi, Russian wolfhound',
     170: 'Irish wolfhound',
     171: 'Italian greyhound',
     172: 'whippet',
     173: 'Ibizan hound, Ibizan Podenco',
     174: 'Norwegian elkhound, elkhound',
     175: 'otterhound, otter hound',
     176: 'Saluki, gazelle hound',
     177: 'Scottish deerhound, deerhound',
     178: 'Weimaraner',
     179: 'Staffordshire bullterrier, Staffordshire bull terrier',
     180: 'American Staffordshire terrier, Staffordshire terrier, American pit bull terrier, pit bull terrier',
     181: 'Bedlington terrier',
     182: 'Border terrier',
     183: 'Kerry blue terrier',
     184: 'Irish terrier',
     185: 'Norfolk terrier',
     186: 'Norwich terrier',
     187: 'Yorkshire terrier',
     188: 'wire-haired fox terrier',
     189: 'Lakeland terrier',
     190: 'Sealyham terrier, Sealyham',
     191: 'Airedale, Airedale terrier',
     192: 'cairn, cairn terrier',
     193: 'Australian terrier',
     194: 'Dandie Dinmont, Dandie Dinmont terrier',
     195: 'Boston bull, Boston terrier',
     196: 'miniature schnauzer',
     197: 'giant schnauzer',
     198: 'standard schnauzer',
     199: 'Scotch terrier, Scottish terrier, Scottie',
     200: 'Tibetan terrier, chrysanthemum dog',
     201: 'silky terrier, Sydney silky',
     202: 'soft-coated wheaten terrier',
     203: 'West Highland white terrier',
     204: 'Lhasa, Lhasa apso',
     205: 'flat-coated retriever',
     206: 'curly-coated retriever',
     207: 'golden retriever',
     208: 'Labrador retriever',
     209: 'Chesapeake Bay retriever',
     210: 'German short-haired pointer',
     211: 'vizsla, Hungarian pointer',
     212: 'English setter',
     213: 'Irish setter, red setter',
     214: 'Gordon setter',
     215: 'Brittany spaniel',
     216: 'clumber, clumber spaniel',
     217: 'English springer, English springer spaniel',
     218: 'Welsh springer spaniel',
     219: 'cocker spaniel, English cocker spaniel, cocker',
     220: 'Sussex spaniel',
     221: 'Irish water spaniel',
     222: 'kuvasz',
     223: 'schipperke',
     224: 'groenendael',
     225: 'malinois',
     226: 'briard',
     227: 'kelpie',
     228: 'komondor',
     229: 'Old English sheepdog, bobtail',
     230: 'Shetland sheepdog, Shetland sheep dog, Shetland',
     231: 'collie',
     232: 'Border collie',
     233: 'Bouvier des Flandres, Bouviers des Flandres',
     234: 'Rottweiler',
     235: 'German shepherd, German shepherd dog, German police dog, alsatian',
     236: 'Doberman, Doberman pinscher',
     237: 'miniature pinscher',
     238: 'Greater Swiss Mountain dog',
     239: 'Bernese mountain dog',
     240: 'Appenzeller',
     241: 'EntleBucher',
     242: 'boxer',
     243: 'bull mastiff',
     244: 'Tibetan mastiff',
     245: 'French bulldog',
     246: 'Great Dane',
     247: 'Saint Bernard, St Bernard',
     248: 'Eskimo dog, husky',
     249: 'malamute, malemute, Alaskan malamute',
     250: 'Siberian husky',
     251: 'dalmatian, coach dog, carriage dog',
     252: 'affenpinscher, monkey pinscher, monkey dog',
     253: 'basenji',
     254: 'pug, pug-dog',
     255: 'Leonberg',
     256: 'Newfoundland, Newfoundland dog',
     257: 'Great Pyrenees',
     258: 'Samoyed, Samoyede',
     259: 'Pomeranian',
     260: 'chow, chow chow',
     261: 'keeshond',
     262: 'Brabancon griffon',
     263: 'Pembroke, Pembroke Welsh corgi',
     264: 'Cardigan, Cardigan Welsh corgi',
     265: 'toy poodle',
     266: 'miniature poodle',
     267: 'standard poodle',
     268: 'Mexican hairless',
     269: 'timber wolf, grey wolf, gray wolf, Canis lupus',
     270: 'white wolf, Arctic wolf, Canis lupus tundrarum',
     271: 'red wolf, maned wolf, Canis rufus, Canis niger',
     272: 'coyote, prairie wolf, brush wolf, Canis latrans',
     273: 'dingo, warrigal, warragal, Canis dingo',
     274: 'dhole, Cuon alpinus',
     275: 'African hunting dog, hyena dog, Cape hunting dog, Lycaon pictus',
     276: 'hyena, hyaena',
     277: 'red fox, Vulpes vulpes',
     278: 'kit fox, Vulpes macrotis',
     279: 'Arctic fox, white fox, Alopex lagopus',
     280: 'grey fox, gray fox, Urocyon cinereoargenteus',
     281: 'tabby, tabby cat',
     282: 'tiger cat',
     283: 'Persian cat',
     284: 'Siamese cat, Siamese',
     285: 'Egyptian cat',
     286: 'cougar, puma, catamount, mountain lion, painter, panther, Felis concolor',
     287: 'lynx, catamount',
     288: 'leopard, Panthera pardus',
     289: 'snow leopard, ounce, Panthera uncia',
     290: 'jaguar, panther, Panthera onca, Felis onca',
     291: 'lion, king of beasts, Panthera leo',
     292: 'tiger, Panthera tigris',
     293: 'cheetah, chetah, Acinonyx jubatus',
     294: 'brown bear, bruin, Ursus arctos',
     295: 'American black bear, black bear, Ursus americanus, Euarctos americanus',
     296: 'ice bear, polar bear, Ursus Maritimus, Thalarctos maritimus',
     297: 'sloth bear, Melursus ursinus, Ursus ursinus',
     298: 'mongoose',
     299: 'meerkat, mierkat',
     300: 'tiger beetle',
     301: 'ladybug, ladybeetle, lady beetle, ladybird, ladybird beetle',
     302: 'ground beetle, carabid beetle',
     303: 'long-horned beetle, longicorn, longicorn beetle',
     304: 'leaf beetle, chrysomelid',
     305: 'dung beetle',
     306: 'rhinoceros beetle',
     307: 'weevil',
     308: 'fly',
     309: 'bee',
     310: 'ant, emmet, pismire',
     311: 'grasshopper, hopper',
     312: 'cricket',
     313: 'walking stick, walkingstick, stick insect',
     314: 'cockroach, roach',
     315: 'mantis, mantid',
     316: 'cicada, cicala',
     317: 'leafhopper',
     318: 'lacewing, lacewing fly',
     319: "dragonfly, darning needle, devil's darning needle, sewing needle, snake feeder, snake doctor, mosquito hawk, skeeter hawk",
     320: 'damselfly',
     321: 'admiral',
     322: 'ringlet, ringlet butterfly',
     323: 'monarch, monarch butterfly, milkweed butterfly, Danaus plexippus',
     324: 'cabbage butterfly',
     325: 'sulphur butterfly, sulfur butterfly',
     326: 'lycaenid, lycaenid butterfly',
     327: 'starfish, sea star',
     328: 'sea urchin',
     329: 'sea cucumber, holothurian',
     330: 'wood rabbit, cottontail, cottontail rabbit',
     331: 'hare',
     332: 'Angora, Angora rabbit',
     333: 'hamster',
     334: 'porcupine, hedgehog',
     335: 'fox squirrel, eastern fox squirrel, Sciurus niger',
     336: 'marmot',
     337: 'beaver',
     338: 'guinea pig, Cavia cobaya',
     339: 'sorrel',
     340: 'zebra',
     341: 'hog, pig, grunter, squealer, Sus scrofa',
     342: 'wild boar, boar, Sus scrofa',
     343: 'warthog',
     344: 'hippopotamus, hippo, river horse, Hippopotamus amphibius',
     345: 'ox',
     346: 'water buffalo, water ox, Asiatic buffalo, Bubalus bubalis',
     347: 'bison',
     348: 'ram, tup',
     349: 'bighorn, bighorn sheep, cimarron, Rocky Mountain bighorn, Rocky Mountain sheep, Ovis canadensis',
     350: 'ibex, Capra ibex',
     351: 'hartebeest',
     352: 'impala, Aepyceros melampus',
     353: 'gazelle',
     354: 'Arabian camel, dromedary, Camelus dromedarius',
     355: 'llama',
     356: 'weasel',
     357: 'mink',
     358: 'polecat, fitch, foulmart, foumart, Mustela putorius',
     359: 'black-footed ferret, ferret, Mustela nigripes',
     360: 'otter',
     361: 'skunk, polecat, wood pussy',
     362: 'badger',
     363: 'armadillo',
     364: 'three-toed sloth, ai, Bradypus tridactylus',
     365: 'orangutan, orang, orangutang, Pongo pygmaeus',
     366: 'gorilla, Gorilla gorilla',
     367: 'chimpanzee, chimp, Pan troglodytes',
     368: 'gibbon, Hylobates lar',
     369: 'siamang, Hylobates syndactylus, Symphalangus syndactylus',
     370: 'guenon, guenon monkey',
     371: 'patas, hussar monkey, Erythrocebus patas',
     372: 'baboon',
     373: 'macaque',
     374: 'langur',
     375: 'colobus, colobus monkey',
     376: 'proboscis monkey, Nasalis larvatus',
     377: 'marmoset',
     378: 'capuchin, ringtail, Cebus capucinus',
     379: 'howler monkey, howler',
     380: 'titi, titi monkey',
     381: 'spider monkey, Ateles geoffroyi',
     382: 'squirrel monkey, Saimiri sciureus',
     383: 'Madagascar cat, ring-tailed lemur, Lemur catta',
     384: 'indri, indris, Indri indri, Indri brevicaudatus',
     385: 'Indian elephant, Elephas maximus',
     386: 'African elephant, Loxodonta africana',
     387: 'lesser panda, red panda, panda, bear cat, cat bear, Ailurus fulgens',
     388: 'giant panda, panda, panda bear, coon bear, Ailuropoda melanoleuca',
     389: 'barracouta, snoek',
     390: 'eel',
     391: 'coho, cohoe, coho salmon, blue jack, silver salmon, Oncorhynchus kisutch',
     392: 'rock beauty, Holocanthus tricolor',
     393: 'anemone fish',
     394: 'sturgeon',
     395: 'gar, garfish, garpike, billfish, Lepisosteus osseus',
     396: 'lionfish',
     397: 'puffer, pufferfish, blowfish, globefish',
     398: 'abacus',
     399: 'abaya',
     400: "academic gown, academic robe, judge's robe",
     401: 'accordion, piano accordion, squeeze box',
     402: 'acoustic guitar',
     403: 'aircraft carrier, carrier, flattop, attack aircraft carrier',
     404: 'airliner',
     405: 'airship, dirigible',
     406: 'altar',
     407: 'ambulance',
     408: 'amphibian, amphibious vehicle',
     409: 'analog clock',
     410: 'apiary, bee house',
     411: 'apron',
     412: 'ashcan, trash can, garbage can, wastebin, ash bin, ash-bin, ashbin, dustbin, trash barrel, trash bin',
     413: 'assault rifle, assault gun',
     414: 'backpack, back pack, knapsack, packsack, rucksack, haversack',
     415: 'bakery, bakeshop, bakehouse',
     416: 'balance beam, beam',
     417: 'balloon',
     418: 'ballpoint, ballpoint pen, ballpen, Biro',
     419: 'Band Aid',
     420: 'banjo',
     421: 'bannister, banister, balustrade, balusters, handrail',
     422: 'barbell',
     423: 'barber chair',
     424: 'barbershop',
     425: 'barn',
     426: 'barometer',
     427: 'barrel, cask',
     428: 'barrow, garden cart, lawn cart, wheelbarrow',
     429: 'baseball',
     430: 'basketball',
     431: 'bassinet',
     432: 'bassoon',
     433: 'bathing cap, swimming cap',
     434: 'bath towel',
     435: 'bathtub, bathing tub, bath, tub',
     436: 'beach wagon, station wagon, wagon, estate car, beach waggon, station waggon, waggon',
     437: 'beacon, lighthouse, beacon light, pharos',
     438: 'beaker',
     439: 'bearskin, busby, shako',
     440: 'beer bottle',
     441: 'beer glass',
     442: 'bell cote, bell cot',
     443: 'bib',
     444: 'bicycle-built-for-two, tandem bicycle, tandem',
     445: 'bikini, two-piece',
     446: 'binder, ring-binder',
     447: 'binoculars, field glasses, opera glasses',
     448: 'birdhouse',
     449: 'boathouse',
     450: 'bobsled, bobsleigh, bob',
     451: 'bolo tie, bolo, bola tie, bola',
     452: 'bonnet, poke bonnet',
     453: 'bookcase',
     454: 'bookshop, bookstore, bookstall',
     455: 'bottlecap',
     456: 'bow',
     457: 'bow tie, bow-tie, bowtie',
     458: 'brass, memorial tablet, plaque',
     459: 'brassiere, bra, bandeau',
     460: 'breakwater, groin, groyne, mole, bulwark, seawall, jetty',
     461: 'breastplate, aegis, egis',
     462: 'broom',
     463: 'bucket, pail',
     464: 'buckle',
     465: 'bulletproof vest',
     466: 'bullet train, bullet',
     467: 'butcher shop, meat market',
     468: 'cab, hack, taxi, taxicab',
     469: 'caldron, cauldron',
     470: 'candle, taper, wax light',
     471: 'cannon',
     472: 'canoe',
     473: 'can opener, tin opener',
     474: 'cardigan',
     475: 'car mirror',
     476: 'carousel, carrousel, merry-go-round, roundabout, whirligig',
     477: "carpenter's kit, tool kit",
     478: 'carton',
     479: 'car wheel',
     480: 'cash machine, cash dispenser, automated teller machine, automatic teller machine, automated teller, automatic teller, ATM',
     481: 'cassette',
     482: 'cassette player',
     483: 'castle',
     484: 'catamaran',
     485: 'CD player',
     486: 'cello, violoncello',
     487: 'cellular telephone, cellular phone, cellphone, cell, mobile phone',
     488: 'chain',
     489: 'chainlink fence',
     490: 'chain mail, ring mail, mail, chain armor, chain armour, ring armor, ring armour',
     491: 'chain saw, chainsaw',
     492: 'chest',
     493: 'chiffonier, commode',
     494: 'chime, bell, gong',
     495: 'china cabinet, china closet',
     496: 'Christmas stocking',
     497: 'church, church building',
     498: 'cinema, movie theater, movie theatre, movie house, picture palace',
     499: 'cleaver, meat cleaver, chopper',
     500: 'cliff dwelling',
     501: 'cloak',
     502: 'clog, geta, patten, sabot',
     503: 'cocktail shaker',
     504: 'coffee mug',
     505: 'coffeepot',
     506: 'coil, spiral, volute, whorl, helix',
     507: 'combination lock',
     508: 'computer keyboard, keypad',
     509: 'confectionery, confectionary, candy store',
     510: 'container ship, containership, container vessel',
     511: 'convertible',
     512: 'corkscrew, bottle screw',
     513: 'cornet, horn, trumpet, trump',
     514: 'cowboy boot',
     515: 'cowboy hat, ten-gallon hat',
     516: 'cradle',
     517: 'crane',
     518: 'crash helmet',
     519: 'crate',
     520: 'crib, cot',
     521: 'Crock Pot',
     522: 'croquet ball',
     523: 'crutch',
     524: 'cuirass',
     525: 'dam, dike, dyke',
     526: 'desk',
     527: 'desktop computer',
     528: 'dial telephone, dial phone',
     529: 'diaper, nappy, napkin',
     530: 'digital clock',
     531: 'digital watch',
     532: 'dining table, board',
     533: 'dishrag, dishcloth',
     534: 'dishwasher, dish washer, dishwashing machine',
     535: 'disk brake, disc brake',
     536: 'dock, dockage, docking facility',
     537: 'dogsled, dog sled, dog sleigh',
     538: 'dome',
     539: 'doormat, welcome mat',
     540: 'drilling platform, offshore rig',
     541: 'drum, membranophone, tympan',
     542: 'drumstick',
     543: 'dumbbell',
     544: 'Dutch oven',
     545: 'electric fan, blower',
     546: 'electric guitar',
     547: 'electric locomotive',
     548: 'entertainment center',
     549: 'envelope',
     550: 'espresso maker',
     551: 'face powder',
     552: 'feather boa, boa',
     553: 'file, file cabinet, filing cabinet',
     554: 'fireboat',
     555: 'fire engine, fire truck',
     556: 'fire screen, fireguard',
     557: 'flagpole, flagstaff',
     558: 'flute, transverse flute',
     559: 'folding chair',
     560: 'football helmet',
     561: 'forklift',
     562: 'fountain',
     563: 'fountain pen',
     564: 'four-poster',
     565: 'freight car',
     566: 'French horn, horn',
     567: 'frying pan, frypan, skillet',
     568: 'fur coat',
     569: 'garbage truck, dustcart',
     570: 'gasmask, respirator, gas helmet',
     571: 'gas pump, gasoline pump, petrol pump, island dispenser',
     572: 'goblet',
     573: 'go-kart',
     574: 'golf ball',
     575: 'golfcart, golf cart',
     576: 'gondola',
     577: 'gong, tam-tam',
     578: 'gown',
     579: 'grand piano, grand',
     580: 'greenhouse, nursery, glasshouse',
     581: 'grille, radiator grille',
     582: 'grocery store, grocery, food market, market',
     583: 'guillotine',
     584: 'hair slide',
     585: 'hair spray',
     586: 'half track',
     587: 'hammer',
     588: 'hamper',
     589: 'hand blower, blow dryer, blow drier, hair dryer, hair drier',
     590: 'hand-held computer, hand-held microcomputer',
     591: 'handkerchief, hankie, hanky, hankey',
     592: 'hard disc, hard disk, fixed disk',
     593: 'harmonica, mouth organ, harp, mouth harp',
     594: 'harp',
     595: 'harvester, reaper',
     596: 'hatchet',
     597: 'holster',
     598: 'home theater, home theatre',
     599: 'honeycomb',
     600: 'hook, claw',
     601: 'hoopskirt, crinoline',
     602: 'horizontal bar, high bar',
     603: 'horse cart, horse-cart',
     604: 'hourglass',
     605: 'iPod',
     606: 'iron, smoothing iron',
     607: "jack-o'-lantern",
     608: 'jean, blue jean, denim',
     609: 'jeep, landrover',
     610: 'jersey, T-shirt, tee shirt',
     611: 'jigsaw puzzle',
     612: 'jinrikisha, ricksha, rickshaw',
     613: 'joystick',
     614: 'kimono',
     615: 'knee pad',
     616: 'knot',
     617: 'lab coat, laboratory coat',
     618: 'ladle',
     619: 'lampshade, lamp shade',
     620: 'laptop, laptop computer',
     621: 'lawn mower, mower',
     622: 'lens cap, lens cover',
     623: 'letter opener, paper knife, paperknife',
     624: 'library',
     625: 'lifeboat',
     626: 'lighter, light, igniter, ignitor',
     627: 'limousine, limo',
     628: 'liner, ocean liner',
     629: 'lipstick, lip rouge',
     630: 'Loafer',
     631: 'lotion',
     632: 'loudspeaker, speaker, speaker unit, loudspeaker system, speaker system',
     633: "loupe, jeweler's loupe",
     634: 'lumbermill, sawmill',
     635: 'magnetic compass',
     636: 'mailbag, postbag',
     637: 'mailbox, letter box',
     638: 'maillot',
     639: 'maillot, tank suit',
     640: 'manhole cover',
     641: 'maraca',
     642: 'marimba, xylophone',
     643: 'mask',
     644: 'matchstick',
     645: 'maypole',
     646: 'maze, labyrinth',
     647: 'measuring cup',
     648: 'medicine chest, medicine cabinet',
     649: 'megalith, megalithic structure',
     650: 'microphone, mike',
     651: 'microwave, microwave oven',
     652: 'military uniform',
     653: 'milk can',
     654: 'minibus',
     655: 'miniskirt, mini',
     656: 'minivan',
     657: 'missile',
     658: 'mitten',
     659: 'mixing bowl',
     660: 'mobile home, manufactured home',
     661: 'Model T',
     662: 'modem',
     663: 'monastery',
     664: 'monitor',
     665: 'moped',
     666: 'mortar',
     667: 'mortarboard',
     668: 'mosque',
     669: 'mosquito net',
     670: 'motor scooter, scooter',
     671: 'mountain bike, all-terrain bike, off-roader',
     672: 'mountain tent',
     673: 'mouse, computer mouse',
     674: 'mousetrap',
     675: 'moving van',
     676: 'muzzle',
     677: 'nail',
     678: 'neck brace',
     679: 'necklace',
     680: 'nipple',
     681: 'notebook, notebook computer',
     682: 'obelisk',
     683: 'oboe, hautboy, hautbois',
     684: 'ocarina, sweet potato',
     685: 'odometer, hodometer, mileometer, milometer',
     686: 'oil filter',
     687: 'organ, pipe organ',
     688: 'oscilloscope, scope, cathode-ray oscilloscope, CRO',
     689: 'overskirt',
     690: 'oxcart',
     691: 'oxygen mask',
     692: 'packet',
     693: 'paddle, boat paddle',
     694: 'paddlewheel, paddle wheel',
     695: 'padlock',
     696: 'paintbrush',
     697: "pajama, pyjama, pj's, jammies",
     698: 'palace',
     699: 'panpipe, pandean pipe, syrinx',
     700: 'paper towel',
     701: 'parachute, chute',
     702: 'parallel bars, bars',
     703: 'park bench',
     704: 'parking meter',
     705: 'passenger car, coach, carriage',
     706: 'patio, terrace',
     707: 'pay-phone, pay-station',
     708: 'pedestal, plinth, footstall',
     709: 'pencil box, pencil case',
     710: 'pencil sharpener',
     711: 'perfume, essence',
     712: 'Petri dish',
     713: 'photocopier',
     714: 'pick, plectrum, plectron',
     715: 'pickelhaube',
     716: 'picket fence, paling',
     717: 'pickup, pickup truck',
     718: 'pier',
     719: 'piggy bank, penny bank',
     720: 'pill bottle',
     721: 'pillow',
     722: 'ping-pong ball',
     723: 'pinwheel',
     724: 'pirate, pirate ship',
     725: 'pitcher, ewer',
     726: "plane, carpenter's plane, woodworking plane",
     727: 'planetarium',
     728: 'plastic bag',
     729: 'plate rack',
     730: 'plow, plough',
     731: "plunger, plumber's helper",
     732: 'Polaroid camera, Polaroid Land camera',
     733: 'pole',
     734: 'police van, police wagon, paddy wagon, patrol wagon, wagon, black Maria',
     735: 'poncho',
     736: 'pool table, billiard table, snooker table',
     737: 'pop bottle, soda bottle',
     738: 'pot, flowerpot',
     739: "potter's wheel",
     740: 'power drill',
     741: 'prayer rug, prayer mat',
     742: 'printer',
     743: 'prison, prison house',
     744: 'projectile, missile',
     745: 'projector',
     746: 'puck, hockey puck',
     747: 'punching bag, punch bag, punching ball, punchball',
     748: 'purse',
     749: 'quill, quill pen',
     750: 'quilt, comforter, comfort, puff',
     751: 'racer, race car, racing car',
     752: 'racket, racquet',
     753: 'radiator',
     754: 'radio, wireless',
     755: 'radio telescope, radio reflector',
     756: 'rain barrel',
     757: 'recreational vehicle, RV, R.V.',
     758: 'reel',
     759: 'reflex camera',
     760: 'refrigerator, icebox',
     761: 'remote control, remote',
     762: 'restaurant, eating house, eating place, eatery',
     763: 'revolver, six-gun, six-shooter',
     764: 'rifle',
     765: 'rocking chair, rocker',
     766: 'rotisserie',
     767: 'rubber eraser, rubber, pencil eraser',
     768: 'rugby ball',
     769: 'rule, ruler',
     770: 'running shoe',
     771: 'safe',
     772: 'safety pin',
     773: 'saltshaker, salt shaker',
     774: 'sandal',
     775: 'sarong',
     776: 'sax, saxophone',
     777: 'scabbard',
     778: 'scale, weighing machine',
     779: 'school bus',
     780: 'schooner',
     781: 'scoreboard',
     782: 'screen, CRT screen',
     783: 'screw',
     784: 'screwdriver',
     785: 'seat belt, seatbelt',
     786: 'sewing machine',
     787: 'shield, buckler',
     788: 'shoe shop, shoe-shop, shoe store',
     789: 'shoji',
     790: 'shopping basket',
     791: 'shopping cart',
     792: 'shovel',
     793: 'shower cap',
     794: 'shower curtain',
     795: 'ski',
     796: 'ski mask',
     797: 'sleeping bag',
     798: 'slide rule, slipstick',
     799: 'sliding door',
     800: 'slot, one-armed bandit',
     801: 'snorkel',
     802: 'snowmobile',
     803: 'snowplow, snowplough',
     804: 'soap dispenser',
     805: 'soccer ball',
     806: 'sock',
     807: 'solar dish, solar collector, solar furnace',
     808: 'sombrero',
     809: 'soup bowl',
     810: 'space bar',
     811: 'space heater',
     812: 'space shuttle',
     813: 'spatula',
     814: 'speedboat',
     815: "spider web, spider's web",
     816: 'spindle',
     817: 'sports car, sport car',
     818: 'spotlight, spot',
     819: 'stage',
     820: 'steam locomotive',
     821: 'steel arch bridge',
     822: 'steel drum',
     823: 'stethoscope',
     824: 'stole',
     825: 'stone wall',
     826: 'stopwatch, stop watch',
     827: 'stove',
     828: 'strainer',
     829: 'streetcar, tram, tramcar, trolley, trolley car',
     830: 'stretcher',
     831: 'studio couch, day bed',
     832: 'stupa, tope',
     833: 'submarine, pigboat, sub, U-boat',
     834: 'suit, suit of clothes',
     835: 'sundial',
     836: 'sunglass',
     837: 'sunglasses, dark glasses, shades',
     838: 'sunscreen, sunblock, sun blocker',
     839: 'suspension bridge',
     840: 'swab, swob, mop',
     841: 'sweatshirt',
     842: 'swimming trunks, bathing trunks',
     843: 'swing',
     844: 'switch, electric switch, electrical switch',
     845: 'syringe',
     846: 'table lamp',
     847: 'tank, army tank, armored combat vehicle, armoured combat vehicle',
     848: 'tape player',
     849: 'teapot',
     850: 'teddy, teddy bear',
     851: 'television, television system',
     852: 'tennis ball',
     853: 'thatch, thatched roof',
     854: 'theater curtain, theatre curtain',
     855: 'thimble',
     856: 'thresher, thrasher, threshing machine',
     857: 'throne',
     858: 'tile roof',
     859: 'toaster',
     860: 'tobacco shop, tobacconist shop, tobacconist',
     861: 'toilet seat',
     862: 'torch',
     863: 'totem pole',
     864: 'tow truck, tow car, wrecker',
     865: 'toyshop',
     866: 'tractor',
     867: 'trailer truck, tractor trailer, trucking rig, rig, articulated lorry, semi',
     868: 'tray',
     869: 'trench coat',
     870: 'tricycle, trike, velocipede',
     871: 'trimaran',
     872: 'tripod',
     873: 'triumphal arch',
     874: 'trolleybus, trolley coach, trackless trolley',
     875: 'trombone',
     876: 'tub, vat',
     877: 'turnstile',
     878: 'typewriter keyboard',
     879: 'umbrella',
     880: 'unicycle, monocycle',
     881: 'upright, upright piano',
     882: 'vacuum, vacuum cleaner',
     883: 'vase',
     884: 'vault',
     885: 'velvet',
     886: 'vending machine',
     887: 'vestment',
     888: 'viaduct',
     889: 'violin, fiddle',
     890: 'volleyball',
     891: 'waffle iron',
     892: 'wall clock',
     893: 'wallet, billfold, notecase, pocketbook',
     894: 'wardrobe, closet, press',
     895: 'warplane, military plane',
     896: 'washbasin, handbasin, washbowl, lavabo, wash-hand basin',
     897: 'washer, automatic washer, washing machine',
     898: 'water bottle',
     899: 'water jug',
     900: 'water tower',
     901: 'whiskey jug',
     902: 'whistle',
     903: 'wig',
     904: 'window screen',
     905: 'window shade',
     906: 'Windsor tie',
     907: 'wine bottle',
     908: 'wing',
     909: 'wok',
     910: 'wooden spoon',
     911: 'wool, woolen, woollen',
     912: 'worm fence, snake fence, snake-rail fence, Virginia fence',
     913: 'wreck',
     914: 'yawl',
     915: 'yurt',
     916: 'web site, website, internet site, site',
     917: 'comic book',
     918: 'crossword puzzle, crossword',
     919: 'street sign',
     920: 'traffic light, traffic signal, stoplight',
     921: 'book jacket, dust cover, dust jacket, dust wrapper',
     922: 'menu',
     923: 'plate',
     924: 'guacamole',
     925: 'consomme',
     926: 'hot pot, hotpot',
     927: 'trifle',
     928: 'ice cream, icecream',
     929: 'ice lolly, lolly, lollipop, popsicle',
     930: 'French loaf',
     931: 'bagel, beigel',
     932: 'pretzel',
     933: 'cheeseburger',
     934: 'hotdog, hot dog, red hot',
     935: 'mashed potato',
     936: 'head cabbage',
     937: 'broccoli',
     938: 'cauliflower',
     939: 'zucchini, courgette',
     940: 'spaghetti squash',
     941: 'acorn squash',
     942: 'butternut squash',
     943: 'cucumber, cuke',
     944: 'artichoke, globe artichoke',
     945: 'bell pepper',
     946: 'cardoon',
     947: 'mushroom',
     948: 'Granny Smith',
     949: 'strawberry',
     950: 'orange',
     951: 'lemon',
     952: 'fig',
     953: 'pineapple, ananas',
     954: 'banana',
     955: 'jackfruit, jak, jack',
     956: 'custard apple',
     957: 'pomegranate',
     958: 'hay',
     959: 'carbonara',
     960: 'chocolate sauce, chocolate syrup',
     961: 'dough',
     962: 'meat loaf, meatloaf',
     963: 'pizza, pizza pie',
     964: 'potpie',
     965: 'burrito',
     966: 'red wine',
     967: 'espresso',
     968: 'cup',
     969: 'eggnog',
     970: 'alp',
     971: 'bubble',
     972: 'cliff, drop, drop-off',
     973: 'coral reef',
     974: 'geyser',
     975: 'lakeside, lakeshore',
     976: 'promontory, headland, head, foreland',
     977: 'sandbar, sand bar',
     978: 'seashore, coast, seacoast, sea-coast',
     979: 'valley, vale',
     980: 'volcano',
     981: 'ballplayer, baseball player',
     982: 'groom, bridegroom',
     983: 'scuba diver',
     984: 'rapeseed',
     985: 'daisy',
     986: "yellow lady's slipper, yellow lady-slipper, Cypripedium calceolus, Cypripedium parviflorum",
     987: 'corn',
     988: 'acorn',
     989: 'hip, rose hip, rosehip',
     990: 'buckeye, horse chestnut, conker',
     991: 'coral fungus',
     992: 'agaric',
     993: 'gyromitra',
     994: 'stinkhorn, carrion fungus',
     995: 'earthstar',
     996: 'hen-of-the-woods, hen of the woods, Polyporus frondosus, Grifola frondosa',
     997: 'bolete',
     998: 'ear, spike, capitulum',
     999: 'toilet tissue, toilet paper, bathroom tissue'}
    


```python
with open(imagenet_filepath, 'rt') as fr:
    index_to_class = eval(fr.read())
    
print(type(index_to_class))
```

    <class 'dict'>
    


```python
index_to_class[10]
```




    'brambling, Fringilla montifringilla'




```python
# 추론할 이미지 다운로드
import requests
from io import BytesIO
from PIL import Image

# img_url = 'https://upload.wikimedia.org/wikipedia/commons/thumb/2/25/Common_goldfish.JPG/800px-Common_goldfish.JPG'
# img_url = 'https://cdn.download.ams.birds.cornell.edu/api/v1/asset/169231441/1800'
# img_url = 'https://blogs.ifas.ufl.edu/news/files/2021/10/anole-FB.jpg'
img_url = 'https://i.pinimg.com/736x/1b/42/50/1b42503a3593d50bec4da04ee69a1b4f.jpg'
res = requests.get(img_url) # url로 요쳥 -> 응답 받기
test_img = Image.open(BytesIO(res.content)) # 응답받은 것이 binary 파일일 경우. BytesIO(binary) => bytes타입 입출력이 가능한 객체
test_img
```




    
![png](output_10_0.png)
    




```python
## import

import torch
from torchvision import models, transforms
from torchinfo import summary

device = 'cuda' if torch.cuda.is_available() else 'cpu'
device
```




    'cpu'




```python
# torchvision에서 제공하는 Pretrained 모델을 다운로드
## VGG19 모델
### weights: 학습된 파라미터들을 같이 다운로드 받는다. => Image Net 데이터셋으로 학습한 파라미터를 받는다.
### IMAGENET1K_V1: Image Net 버전 1로 학습된 파라미터
### IMAGENET1K_V2: Image Net 버전 2로 학습된 파라미터
#### 모델에 따라서 V2는 없을 수도 있다.
##### VGG19_Weights.DEFAULT - 둘중에 기본 데이터셋으로 설정된 것을 받는다.
load_model_vgg = models.vgg19(weights=models.VGG19_Weights.IMAGENET1K_V1)
```


```python
load_model_alex = models.alexnet(weights=models.AlexNet_Weights.DEFAULT)
```


```python
summary(load_model_vgg, (100, 3, 224, 224))
```




    ==========================================================================================
    Layer (type:depth-idx)                   Output Shape              Param #
    ==========================================================================================
    VGG                                      [100, 1000]               --
    ├─Sequential: 1-1                        [100, 512, 7, 7]          --
    │    └─Conv2d: 2-1                       [100, 64, 224, 224]       1,792
    │    └─ReLU: 2-2                         [100, 64, 224, 224]       --
    │    └─Conv2d: 2-3                       [100, 64, 224, 224]       36,928
    │    └─ReLU: 2-4                         [100, 64, 224, 224]       --
    │    └─MaxPool2d: 2-5                    [100, 64, 112, 112]       --
    │    └─Conv2d: 2-6                       [100, 128, 112, 112]      73,856
    │    └─ReLU: 2-7                         [100, 128, 112, 112]      --
    │    └─Conv2d: 2-8                       [100, 128, 112, 112]      147,584
    │    └─ReLU: 2-9                         [100, 128, 112, 112]      --
    │    └─MaxPool2d: 2-10                   [100, 128, 56, 56]        --
    │    └─Conv2d: 2-11                      [100, 256, 56, 56]        295,168
    │    └─ReLU: 2-12                        [100, 256, 56, 56]        --
    │    └─Conv2d: 2-13                      [100, 256, 56, 56]        590,080
    │    └─ReLU: 2-14                        [100, 256, 56, 56]        --
    │    └─Conv2d: 2-15                      [100, 256, 56, 56]        590,080
    │    └─ReLU: 2-16                        [100, 256, 56, 56]        --
    │    └─Conv2d: 2-17                      [100, 256, 56, 56]        590,080
    │    └─ReLU: 2-18                        [100, 256, 56, 56]        --
    │    └─MaxPool2d: 2-19                   [100, 256, 28, 28]        --
    │    └─Conv2d: 2-20                      [100, 512, 28, 28]        1,180,160
    │    └─ReLU: 2-21                        [100, 512, 28, 28]        --
    │    └─Conv2d: 2-22                      [100, 512, 28, 28]        2,359,808
    │    └─ReLU: 2-23                        [100, 512, 28, 28]        --
    │    └─Conv2d: 2-24                      [100, 512, 28, 28]        2,359,808
    │    └─ReLU: 2-25                        [100, 512, 28, 28]        --
    │    └─Conv2d: 2-26                      [100, 512, 28, 28]        2,359,808
    │    └─ReLU: 2-27                        [100, 512, 28, 28]        --
    │    └─MaxPool2d: 2-28                   [100, 512, 14, 14]        --
    │    └─Conv2d: 2-29                      [100, 512, 14, 14]        2,359,808
    │    └─ReLU: 2-30                        [100, 512, 14, 14]        --
    │    └─Conv2d: 2-31                      [100, 512, 14, 14]        2,359,808
    │    └─ReLU: 2-32                        [100, 512, 14, 14]        --
    │    └─Conv2d: 2-33                      [100, 512, 14, 14]        2,359,808
    │    └─ReLU: 2-34                        [100, 512, 14, 14]        --
    │    └─Conv2d: 2-35                      [100, 512, 14, 14]        2,359,808
    │    └─ReLU: 2-36                        [100, 512, 14, 14]        --
    │    └─MaxPool2d: 2-37                   [100, 512, 7, 7]          --
    ├─AdaptiveAvgPool2d: 1-2                 [100, 512, 7, 7]          --
    ├─Sequential: 1-3                        [100, 1000]               --
    │    └─Linear: 2-38                      [100, 4096]               102,764,544
    │    └─ReLU: 2-39                        [100, 4096]               --
    │    └─Dropout: 2-40                     [100, 4096]               --
    │    └─Linear: 2-41                      [100, 4096]               16,781,312
    │    └─ReLU: 2-42                        [100, 4096]               --
    │    └─Dropout: 2-43                     [100, 4096]               --
    │    └─Linear: 2-44                      [100, 1000]               4,097,000
    ==========================================================================================
    Total params: 143,667,240
    Trainable params: 143,667,240
    Non-trainable params: 0
    Total mult-adds (T): 1.96
    ==========================================================================================
    Input size (MB): 60.21
    Forward/backward pass size (MB): 11889.03
    Params size (MB): 574.67
    Estimated Total Size (MB): 12523.91
    ==========================================================================================




```python
summary(load_model_alex, (1, 3, 224, 224))
```




    ==========================================================================================
    Layer (type:depth-idx)                   Output Shape              Param #
    ==========================================================================================
    AlexNet                                  [1, 1000]                 --
    ├─Sequential: 1-1                        [1, 256, 6, 6]            --
    │    └─Conv2d: 2-1                       [1, 64, 55, 55]           23,296
    │    └─ReLU: 2-2                         [1, 64, 55, 55]           --
    │    └─MaxPool2d: 2-3                    [1, 64, 27, 27]           --
    │    └─Conv2d: 2-4                       [1, 192, 27, 27]          307,392
    │    └─ReLU: 2-5                         [1, 192, 27, 27]          --
    │    └─MaxPool2d: 2-6                    [1, 192, 13, 13]          --
    │    └─Conv2d: 2-7                       [1, 384, 13, 13]          663,936
    │    └─ReLU: 2-8                         [1, 384, 13, 13]          --
    │    └─Conv2d: 2-9                       [1, 256, 13, 13]          884,992
    │    └─ReLU: 2-10                        [1, 256, 13, 13]          --
    │    └─Conv2d: 2-11                      [1, 256, 13, 13]          590,080
    │    └─ReLU: 2-12                        [1, 256, 13, 13]          --
    │    └─MaxPool2d: 2-13                   [1, 256, 6, 6]            --
    ├─AdaptiveAvgPool2d: 1-2                 [1, 256, 6, 6]            --
    ├─Sequential: 1-3                        [1, 1000]                 --
    │    └─Dropout: 2-14                     [1, 9216]                 --
    │    └─Linear: 2-15                      [1, 4096]                 37,752,832
    │    └─ReLU: 2-16                        [1, 4096]                 --
    │    └─Dropout: 2-17                     [1, 4096]                 --
    │    └─Linear: 2-18                      [1, 4096]                 16,781,312
    │    └─ReLU: 2-19                        [1, 4096]                 --
    │    └─Linear: 2-20                      [1, 1000]                 4,097,000
    ==========================================================================================
    Total params: 61,100,840
    Trainable params: 61,100,840
    Non-trainable params: 0
    Total mult-adds (M): 714.68
    ==========================================================================================
    Input size (MB): 0.60
    Forward/backward pass size (MB): 3.95
    Params size (MB): 244.40
    Estimated Total Size (MB): 248.96
    ==========================================================================================




```python
device
```




    'cpu'




```python
model = load_model_alex.to(device)
i = torch.randn(1, 3, 224, 224).to(device)
p = model(i)
p.shape
```




    torch.Size([1, 1000])




```python
label = p[0].argmax(dim=-1).item()
index_to_class[label]
```




    'poncho'




```python
torch.nn.Softmax(dim=-1)(p[0]).max(dim=-1).values
```




    tensor(0.1020, grad_fn=<MaxBackward0>)




```python
# img_url = 'https://upload.wikimedia.org/wikipedia/commons/thumb/2/25/Common_goldfish.JPG/800px-Common_goldfish.JPG'
# img_url = 'https://cdn.download.ams.birds.cornell.edu/api/v1/asset/169231441/1800'
# img_url = 'https://blogs.ifas.ufl.edu/news/files/2021/10/anole-FB.jpg'
img_url = 'https://i.pinimg.com/736x/1b/42/50/1b42503a3593d50bec4da04ee69a1b4f.jpg'

res = requests.get(img_url) # url로 요쳥 -> 응답 받기
test_img = Image.open(BytesIO(res.content)) # 응답받은 것이 binary 파일일 경우. BytesIO(binary) => bytes타입 입출력이 가능한 객체
test_img
```




    
![png](output_20_0.png)
    




```python
# 전처리 
test_transform = transforms.Compose([
    transforms.Resize((224, 224), antialias=True),
    transforms.ToTensor() 
    # PIL,Image -> torch.Tensor, 0 ~ 1 scaling, channel first 처리
])
```


```python
input_data = test_transform(test_img)
print(type(input_data), input_data.shape, input_data.min(), input_data.max())
```

    <class 'torch.Tensor'> torch.Size([3, 224, 224]) tensor(0.) tensor(1.)
    


```python
# batch 축을 추가
input_data = input_data.unsqueeze(dim=0)
input_data.shape
```




    torch.Size([1, 3, 224, 224])




```python
# 모델을 device로 이동
model = load_model_vgg.to(device)
```


```python
# 추론
pred1 = model(input_data.to(device))
```


```python
# 추론결과를 확률로 변환
pred_prob1 = torch.nn.Softmax(dim=-1)(pred1)
# 추론 label, 확률값을 조회
label1 = pred_prob1.max(dim=-1).indices
prob1 = pred_prob1.max(dim=-1).values
# 추론 label을 이용해서 label name을 조회
labelname1 = index_to_class[label1.item()]
```


```python
print(label1.item(), labelname1)
print('확률:', prob1.item())
```

    283 Persian cat
    확률: 0.6492896676063538
    

# Transfer learning (전이학습)
- 사전에 학습된 신경망의 구조와 파라미터를 재사용해서 새로운 모델(우리가 만드는 모델)의 시작점으로 삼고 해결하려는 문제를 위해 다시 학습시킨다.
- 전이 학습을 통해 다음을 해결할 수 있다.
    1. 데이터 부족문제
        - 딥러닝은 대용량의 학습데이터가 필요하다.
        - 충분한 데이터를 수집하는 것은 항상 어렵다.
    2. 과다한 계산량
        - 신경망 학습에는 엄청난 양의 계산 자원이 필요하다.

![transfer_learning01](figures/09_transfer_01.png)

- 미리 학습된(pre-trained) Model을 이용하여 모델을 구성한 뒤 현재 하려는 예측 문제를 해결한다.
- 보통 Pretrained Model에서 Feature Extraction 부분을 사용한다.
    - Computer Vision 문제의 경우 Bottom 쪽의 Convolution Layer(Feature Extractor)들은 이미지에 나타나는 일반적인 특성을 추출하므로 **다른 대상을 가지고 학습했다고 하더라도 재사용할 수 있다.**
    - Top 부분 Layer 부분은 특히 출력 Layer의 경우 대상 데이터셋의 목적에 맞게 변경 해야 하므로 재사용할 수 없다.

![transfer_learning02](figures/09_transfer_02.png)

> **Frozon**: Training시 parameter가 update 되지 않도록 하는 것을 말한다.

### Feature extraction 재사용
- Pretrained Model에서 Feature Extractor 만 가져오고 추론기(Fully connected layer)만 새로 정의한 뒤 그 둘을 합쳐서 모델을 만든다.
- 학습시 직접 구성한 추론기만 학습되도록 한다.
    - Feature Extractor는 추론을 위한 Feature 추출을 하는 역할만 하고 그 parameter(weight)가 학습되지 않도록 한다.
- 모델/레이어의 parameter trainable 여부 속성 변경
    - model/layer 의 `parameters()` 메소드를 이용해 weight와 bias를 조회한 뒤 `requires_grad` 속성을 `False`로 변경한다.
        
#### Backbone, Base network
전체 네트워크에서 Feature Extraction의 역할을 담당하는 부분을 backbone/base network라고 한다.

## Fine-tuning(미세조정)
- Transfer Learning을 위한 Pretrained 모델을 내가 학습시켜야 하는 데이터셋(Custom Dataset)으로 **재학습**시키는 것을 fine tunning 이라고 한다.
- 주어진 문제에 더 적합하도록 Feature Extractor의 가중치들도 조정 한다.

### Fine tuning 전략
![transfer02](figures/09_transfer_03.png)

- **세 전략 모두 추론기는 trainable로 한다.**

**<font size='5'>1. 전체 모델을 전부 학습시킨다.(1번)</font>**    
- Pretrained 모델의 weight는 Feature extraction 의 초기 weight 역할을 한다.
- **Train dataset의 양이 많고** Pretrained 모델이 학습했던 dataset과 Custom dataset의 class간의 유사성이 **낮은 경우** 적용.
- 학습에 시간이 많이 걸린다.
    
    
**<font size='5'>2. Pretrained 모델 Bottom layer들(Input과 가까운 Layer들)은 고정시키고 Top layer의 일부를 재학습시킨다.(2번)</font>**     
- **Train dataset의 양이 많고** Pretrained 모델이 학습했던 dataset과 Custom dataset의 class간의 유사성이 **높은 경우** 적용.
- **Train dataset의 양이 적고** Pretained 모델이 학습했던 dataset과 custom dataset의 class간의 유사성이 **낮은 경우** 적용
    
    
**<font size='5'>3. Pretrained 모델 전체를 고정시키고 classifier layer들만 학습시킨다.(3번)</font>**      
- **Train dataset의 양이 적고** Pretrained 모델이 학습했던 dataset과 Custom dataset의 class간의 유사성이 **높은 경우** 적용.
  
  
> **Custom dataset:** 내가 학습시키고자 하는 dataset 
> 1번 2번 전략을 Fine tuning 이라고 한다.

![fine tuning](figures/09_finetuning.png)


```python
%%writefile module/utils.py

import matplotlib.pyplot as plt

def plot_fit_result(train_loss_list, train_acc_list, val_loss_list, val_acc_list):
    plt.figure(figsize=(12, 5))
    plt.subplot(1, 2, 1)
    plt.title('Loss')
    plt.plot(train_loss_list, label='Train')
    plt.plot(val_loss_list, label='Validation')
    plt.xlabel('Epoch')
    plt.ylabel('Loss')
    plt.legend()

    plt.subplot(1, 2, 2)
    plt.title('Accuracy')
    plt.plot(train_acc_list, label=Train)
    plt.plot(val_acc_list, label='Validation')
    plt.xlabel('Epoch')
    plt.ylabel('Accuracy')
    plt.legend()

    plt.tight_layout()
    plt.show()
```

    Overwriting module/utils.py
    


```python
import torch
import torch.nn as nn
import torch.optim as optim
from torch.utils.data import DataLoader
from torchvision import models, datasets, transforms
from torchinfo import summary

from module.train import fit
from module.utils import plot_fit_result
import matplotlib.pyplot as plt

import os
from zipfile import ZipFile # zip 압축 풀기 압축하기
!pip install gdown -U
import gdown

device = 'cuda' if torch.cuda.is_available() else "cpu"
device
```

    Requirement already satisfied: gdown in c:\users\world\anaconda3\envs\torch\lib\site-packages (4.7.1)
    Requirement already satisfied: filelock in c:\users\world\anaconda3\envs\torch\lib\site-packages (from gdown) (3.12.4)
    Requirement already satisfied: requests[socks] in c:\users\world\anaconda3\envs\torch\lib\site-packages (from gdown) (2.31.0)
    Requirement already satisfied: six in c:\users\world\anaconda3\envs\torch\lib\site-packages (from gdown) (1.16.0)
    Requirement already satisfied: tqdm in c:\users\world\anaconda3\envs\torch\lib\site-packages (from gdown) (4.66.1)
    Requirement already satisfied: beautifulsoup4 in c:\users\world\anaconda3\envs\torch\lib\site-packages (from gdown) (4.12.2)
    Requirement already satisfied: soupsieve>1.2 in c:\users\world\anaconda3\envs\torch\lib\site-packages (from beautifulsoup4->gdown) (2.5)
    Requirement already satisfied: charset-normalizer<4,>=2 in c:\users\world\anaconda3\envs\torch\lib\site-packages (from requests[socks]->gdown) (3.3.0)
    Requirement already satisfied: idna<4,>=2.5 in c:\users\world\anaconda3\envs\torch\lib\site-packages (from requests[socks]->gdown) (3.4)
    Requirement already satisfied: urllib3<3,>=1.21.1 in c:\users\world\anaconda3\envs\torch\lib\site-packages (from requests[socks]->gdown) (2.0.6)
    Requirement already satisfied: certifi>=2017.4.17 in c:\users\world\anaconda3\envs\torch\lib\site-packages (from requests[socks]->gdown) (2023.7.22)
    Requirement already satisfied: PySocks!=1.5.7,>=1.5.6 in c:\users\world\anaconda3\envs\torch\lib\site-packages (from requests[socks]->gdown) (1.7.1)
    Requirement already satisfied: colorama in c:\users\world\anaconda3\envs\torch\lib\site-packages (from tqdm->gdown) (0.4.6)
    




    'cpu'




```python
# download
url = 'https://drive.google.com/uc?id=1YIxDL0XJhhAMdScdRUfDgccAqyCw5-ZV'
path = r'datasets/cats_and_dogs_small.zip'
gdown.download(url, path, quiet=False)
```

    Downloading...
    From (uriginal): https://drive.google.com/uc?id=1YIxDL0XJhhAMdScdRUfDgccAqyCw5-ZV
    From (redirected): https://drive.google.com/uc?id=1YIxDL0XJhhAMdScdRUfDgccAqyCw5-ZV&confirm=t&uuid=4d6fd525-1298-477e-90b9-c8581f4df9b5
    To: C:\Users\world\Downloads\07_Deeplearning_pytorch\datasets\cats_and_dogs_small.zip
    100%|█████████████████████████████████████████████████████████████████████████████| 90.8M/90.8M [00:05<00:00, 16.1MB/s]
    




    'datasets/cats_and_dogs_small.zip'




```python
# 압축 풀기 - zip
image_path = os.path.join('datasets', 'cats_and_dogs_small') # 압축 풀 경로
with ZipFile(path) as zfile: # Zipfile(압축풀경로)
    zfile.extractall(image_path) # extractall(압축풀 디렉토리) # 경로생략시 - 현재 DIR에 푼다.
```

## Dataset, Dataloader 생성



```python
# transform 정의 
## Trainset: Image augmentation + Resize + ToTensor
train_transform = transforms.Compose([
    transforms.Resize((224, 224)),    # Image Net 학습 모델 => input size: 224, 224
    # 상수  -> 1-상수 ~ 1+상수 범위 내에서 변환
    transforms.ColorJitter(brightness=0.3, contrast=0.3, saturation=0.5, hue=0.15),
    transforms.RandomHorizontalFlip(), # p=0.5(Default)
    transforms.RandomVerticalFlip(),
    transforms.RandomRotation(degrees=90), # - 90 ~ +90 범위에서 회전
    transforms.ToTensor(),
    transforms.Normalize((0.485, 0.456, 0.406), (0.229, 0.224, 0.225))
    # 채널별: (평균), (표준편차) # (각픽셀값-평균)/(표준편차)
])
```


```python
# validation/test set ==> Image augmentation은 정의하지 않는다
## Resize, ToTensor, Noirmalize
test_transform = transforms.Compose([
    transforms.Resize((224, 224)),
    transforms.ToTensor(),
    transforms.Normalize((0.485, 0.456, 0.406), (0.229, 0.224, 0.225))
])
```

#### Dataset


```python
train_set = datasets.ImageFolder(os.path.join(image_path, 'train'), transform=train_transform)
valid_set = datasets.ImageFolder(os.path.join(image_path, 'validation'), transform=test_transform)
test_set = datasets.ImageFolder(os.path.join(image_path, 'test'), transform=test_transform)
```


```python
train_set.classes
```




    ['cats', 'dogs']




```python
train_set.class_to_idx
```




    {'cats': 0, 'dogs': 1}




```python
len(train_set), len(valid_set), len(test_set)
```




    (2000, 1000, 1000)




```python
# 하이퍼 파라미터
BATCH_SIZE = 256
N_EPOCH = 1
LR = 0.001
```


```python
# Dataloader
train_loader = DataLoader(train_set, batch_size=BATCH_SIZE, drop_last=True, shuffle=True, 
                          num_workers=os.cpu_count() # Data를 laod 할때 병렬 처리하겠다. => 한번에 몇개씩 동시에 loading 할지 
                          # os.cpu_count() -> cpu 프로세스 개수
                         )
test_loader = DataLoader(test_set, batch_size=BATCH_SIZE, num_workers=os.cpu_count())
valid_loader = DataLoader(valid_set, batch_size=BATCH_SIZE, num_workers=os.cpu_count())
```


```python
# step 수
len(train_loader), len(test_loader), len(valid_loader)
```




    (7, 4, 4)




```python
# 한 배치만 조회
batch_one = next(iter(train_loader)) # (X, y)
batch_one[0].shape, batch_one[1].shape
```




    (torch.Size([256, 3, 224, 224]), torch.Size([256]))




```python
plt.imshow(batch_one[0][0].permute(1, 2, 0))
plt.show()
```

    Clipping input data to the valid range for imshow with RGB data ([0..1] for floats or [0..255] for integers).
    


    
![png](output_52_1.png)
    



```python
# layer 의 파라미터들을 조회 -> model.parameters(): 모델의 모든 layer들의 모든 파라미터(weight, bias)를 반환하는 gennerator
## layer의 파라미터 -> layer객체.weight, layer객체.bias

# 파라미터(Tensor)의 train가능 여부 -> Tensor객체.requres_grad: True - Trainable, False: Non-Trainable(Frozen)
```


```python
# pretrained VGG19 모델
model = models.vgg19(weights=models.VGG19_Weights.DEFAULT)
```


```python
# 파라미터 확인 - trainable 상태를 확인
for param in model.parameters():
    print(param.shape, param.requires_grad)
    break
```

    torch.Size([64, 3, 3, 3]) True
    


```python
# VGG19의 parameter 들을 frozen(학습이 안되도록 처리) -> optimizer.step()시 업데이트 안되도록한다.
## requires_grad=False
for param in model.parameters():
    param.requires_grad = False
```


```python
for param in model.parameters():
    print(param.shape, param.requires_grad)
    break
```

    torch.Size([64, 3, 3, 3]) False
    


```python
summary(model, (1, 3, 224, 224))
```




    ==========================================================================================
    Layer (type:depth-idx)                   Output Shape              Param #
    ==========================================================================================
    VGG                                      [1, 1000]                 --
    ├─Sequential: 1-1                        [1, 512, 7, 7]            --
    │    └─Conv2d: 2-1                       [1, 64, 224, 224]         (1,792)
    │    └─ReLU: 2-2                         [1, 64, 224, 224]         --
    │    └─Conv2d: 2-3                       [1, 64, 224, 224]         (36,928)
    │    └─ReLU: 2-4                         [1, 64, 224, 224]         --
    │    └─MaxPool2d: 2-5                    [1, 64, 112, 112]         --
    │    └─Conv2d: 2-6                       [1, 128, 112, 112]        (73,856)
    │    └─ReLU: 2-7                         [1, 128, 112, 112]        --
    │    └─Conv2d: 2-8                       [1, 128, 112, 112]        (147,584)
    │    └─ReLU: 2-9                         [1, 128, 112, 112]        --
    │    └─MaxPool2d: 2-10                   [1, 128, 56, 56]          --
    │    └─Conv2d: 2-11                      [1, 256, 56, 56]          (295,168)
    │    └─ReLU: 2-12                        [1, 256, 56, 56]          --
    │    └─Conv2d: 2-13                      [1, 256, 56, 56]          (590,080)
    │    └─ReLU: 2-14                        [1, 256, 56, 56]          --
    │    └─Conv2d: 2-15                      [1, 256, 56, 56]          (590,080)
    │    └─ReLU: 2-16                        [1, 256, 56, 56]          --
    │    └─Conv2d: 2-17                      [1, 256, 56, 56]          (590,080)
    │    └─ReLU: 2-18                        [1, 256, 56, 56]          --
    │    └─MaxPool2d: 2-19                   [1, 256, 28, 28]          --
    │    └─Conv2d: 2-20                      [1, 512, 28, 28]          (1,180,160)
    │    └─ReLU: 2-21                        [1, 512, 28, 28]          --
    │    └─Conv2d: 2-22                      [1, 512, 28, 28]          (2,359,808)
    │    └─ReLU: 2-23                        [1, 512, 28, 28]          --
    │    └─Conv2d: 2-24                      [1, 512, 28, 28]          (2,359,808)
    │    └─ReLU: 2-25                        [1, 512, 28, 28]          --
    │    └─Conv2d: 2-26                      [1, 512, 28, 28]          (2,359,808)
    │    └─ReLU: 2-27                        [1, 512, 28, 28]          --
    │    └─MaxPool2d: 2-28                   [1, 512, 14, 14]          --
    │    └─Conv2d: 2-29                      [1, 512, 14, 14]          (2,359,808)
    │    └─ReLU: 2-30                        [1, 512, 14, 14]          --
    │    └─Conv2d: 2-31                      [1, 512, 14, 14]          (2,359,808)
    │    └─ReLU: 2-32                        [1, 512, 14, 14]          --
    │    └─Conv2d: 2-33                      [1, 512, 14, 14]          (2,359,808)
    │    └─ReLU: 2-34                        [1, 512, 14, 14]          --
    │    └─Conv2d: 2-35                      [1, 512, 14, 14]          (2,359,808)
    │    └─ReLU: 2-36                        [1, 512, 14, 14]          --
    │    └─MaxPool2d: 2-37                   [1, 512, 7, 7]            --
    ├─AdaptiveAvgPool2d: 1-2                 [1, 512, 7, 7]            --
    ├─Sequential: 1-3                        [1, 1000]                 --
    │    └─Linear: 2-38                      [1, 4096]                 (102,764,544)
    │    └─ReLU: 2-39                        [1, 4096]                 --
    │    └─Dropout: 2-40                     [1, 4096]                 --
    │    └─Linear: 2-41                      [1, 4096]                 (16,781,312)
    │    └─ReLU: 2-42                        [1, 4096]                 --
    │    └─Dropout: 2-43                     [1, 4096]                 --
    │    └─Linear: 2-44                      [1, 1000]                 (4,097,000)
    ==========================================================================================
    Total params: 143,667,240
    Trainable params: 0
    Non-trainable params: 143,667,240
    Total mult-adds (G): 19.65
    ==========================================================================================
    Input size (MB): 0.60
    Forward/backward pass size (MB): 118.89
    Params size (MB): 574.67
    Estimated Total Size (MB): 694.16
    ==========================================================================================




```python
print(model) # instance변수(attribute)들을 출력
```

    VGG(
      (features): Sequential(
        (0): Conv2d(3, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (1): ReLU(inplace=True)
        (2): Conv2d(64, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (3): ReLU(inplace=True)
        (4): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
        (5): Conv2d(64, 128, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (6): ReLU(inplace=True)
        (7): Conv2d(128, 128, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (8): ReLU(inplace=True)
        (9): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
        (10): Conv2d(128, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (11): ReLU(inplace=True)
        (12): Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (13): ReLU(inplace=True)
        (14): Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (15): ReLU(inplace=True)
        (16): Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (17): ReLU(inplace=True)
        (18): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
        (19): Conv2d(256, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (20): ReLU(inplace=True)
        (21): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (22): ReLU(inplace=True)
        (23): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (24): ReLU(inplace=True)
        (25): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (26): ReLU(inplace=True)
        (27): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
        (28): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (29): ReLU(inplace=True)
        (30): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (31): ReLU(inplace=True)
        (32): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (33): ReLU(inplace=True)
        (34): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (35): ReLU(inplace=True)
        (36): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
      )
      (avgpool): AdaptiveAvgPool2d(output_size=(7, 7))
      (classifier): Sequential(
        (0): Linear(in_features=25088, out_features=4096, bias=True)
        (1): ReLU(inplace=True)
        (2): Dropout(p=0.5, inplace=False)
        (3): Linear(in_features=4096, out_features=4096, bias=True)
        (4): ReLU(inplace=True)
        (5): Dropout(p=0.5, inplace=False)
        (6): Linear(in_features=4096, out_features=1000, bias=True)
      )
    )
    


```python
# classifer를 정의해서 model에 추가
## VGG19의 atrribute 변수 classifier에 추론기가 정의 설정되 있는 상태 => 새로운 추론기(개/고양이 분류)을 정의한뒤 classifier attribute 변수에 할당
model.classifier = nn.Sequential(
    nn.Linear(512 * 7 * 7, 4096),
    nn.ReLU(),
    nn.Dropout(p=0.3),
    
    nn.Linear(4096, 4096),
    nn.ReLU(),
    nn.Dropout(p=0.3),
    
    nn.Linear(4096, 2), # 개 / 고양이 두개 class 를 분류 => out: 2
)
```


```python
summary(model, (1, 3, 224, 224))
```




    ==========================================================================================
    Layer (type:depth-idx)                   Output Shape              Param #
    ==========================================================================================
    VGG                                      [1, 2]                    --
    ├─Sequential: 1-1                        [1, 512, 7, 7]            --
    │    └─Conv2d: 2-1                       [1, 64, 224, 224]         (1,792)
    │    └─ReLU: 2-2                         [1, 64, 224, 224]         --
    │    └─Conv2d: 2-3                       [1, 64, 224, 224]         (36,928)
    │    └─ReLU: 2-4                         [1, 64, 224, 224]         --
    │    └─MaxPool2d: 2-5                    [1, 64, 112, 112]         --
    │    └─Conv2d: 2-6                       [1, 128, 112, 112]        (73,856)
    │    └─ReLU: 2-7                         [1, 128, 112, 112]        --
    │    └─Conv2d: 2-8                       [1, 128, 112, 112]        (147,584)
    │    └─ReLU: 2-9                         [1, 128, 112, 112]        --
    │    └─MaxPool2d: 2-10                   [1, 128, 56, 56]          --
    │    └─Conv2d: 2-11                      [1, 256, 56, 56]          (295,168)
    │    └─ReLU: 2-12                        [1, 256, 56, 56]          --
    │    └─Conv2d: 2-13                      [1, 256, 56, 56]          (590,080)
    │    └─ReLU: 2-14                        [1, 256, 56, 56]          --
    │    └─Conv2d: 2-15                      [1, 256, 56, 56]          (590,080)
    │    └─ReLU: 2-16                        [1, 256, 56, 56]          --
    │    └─Conv2d: 2-17                      [1, 256, 56, 56]          (590,080)
    │    └─ReLU: 2-18                        [1, 256, 56, 56]          --
    │    └─MaxPool2d: 2-19                   [1, 256, 28, 28]          --
    │    └─Conv2d: 2-20                      [1, 512, 28, 28]          (1,180,160)
    │    └─ReLU: 2-21                        [1, 512, 28, 28]          --
    │    └─Conv2d: 2-22                      [1, 512, 28, 28]          (2,359,808)
    │    └─ReLU: 2-23                        [1, 512, 28, 28]          --
    │    └─Conv2d: 2-24                      [1, 512, 28, 28]          (2,359,808)
    │    └─ReLU: 2-25                        [1, 512, 28, 28]          --
    │    └─Conv2d: 2-26                      [1, 512, 28, 28]          (2,359,808)
    │    └─ReLU: 2-27                        [1, 512, 28, 28]          --
    │    └─MaxPool2d: 2-28                   [1, 512, 14, 14]          --
    │    └─Conv2d: 2-29                      [1, 512, 14, 14]          (2,359,808)
    │    └─ReLU: 2-30                        [1, 512, 14, 14]          --
    │    └─Conv2d: 2-31                      [1, 512, 14, 14]          (2,359,808)
    │    └─ReLU: 2-32                        [1, 512, 14, 14]          --
    │    └─Conv2d: 2-33                      [1, 512, 14, 14]          (2,359,808)
    │    └─ReLU: 2-34                        [1, 512, 14, 14]          --
    │    └─Conv2d: 2-35                      [1, 512, 14, 14]          (2,359,808)
    │    └─ReLU: 2-36                        [1, 512, 14, 14]          --
    │    └─MaxPool2d: 2-37                   [1, 512, 7, 7]            --
    ├─AdaptiveAvgPool2d: 1-2                 [1, 512, 7, 7]            --
    ├─Sequential: 1-3                        [1, 2]                    --
    │    └─Linear: 2-38                      [1, 4096]                 102,764,544
    │    └─ReLU: 2-39                        [1, 4096]                 --
    │    └─Dropout: 2-40                     [1, 4096]                 --
    │    └─Linear: 2-41                      [1, 4096]                 16,781,312
    │    └─ReLU: 2-42                        [1, 4096]                 --
    │    └─Dropout: 2-43                     [1, 4096]                 --
    │    └─Linear: 2-44                      [1, 2]                    8,194
    ==========================================================================================
    Total params: 139,578,434
    Trainable params: 119,554,050
    Non-trainable params: 20,024,384
    Total mult-adds (G): 19.64
    ==========================================================================================
    Input size (MB): 0.60
    Forward/backward pass size (MB): 118.88
    Params size (MB): 558.31
    Estimated Total Size (MB): 677.80
    ==========================================================================================




```python
# 학습
## 코랩으로 
model = model.to(device)
loss_fn = nn.CrossEntropyLoss()
optimizer = optim.Adam(model.parameters(), lr=LR)
result = fit(train_loader, valid_loader, model, loss_fn, optimizer, N_EPOCH, save_best_model=False, device=device, mode='multi')
```
