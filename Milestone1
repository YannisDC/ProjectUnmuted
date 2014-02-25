from struct import pack
from math import pow
from math import pi
from math import sin
from math import trunc
from string import maketrans

List_frequencies=[]
List_volumesIn_dB=[]
my_list=[]

array = []
helparray = []

Time = float(raw_input("How long did you want the sound to last in seconds?:"))
NumChannels = int(raw_input("How many channels? (mono=1 stereo=2):"))
SampleRate = 22050
print "Samplerate=" + str(SampleRate)
BitsPerSample = 16
BlockAlign = (NumChannels * (BitsPerSample/8))
ByteRate = (SampleRate * BlockAlign)
Subchunk2Size = (Time * ByteRate)


source = open('spectrum.txt','r')
array = source.readlines()
AmountOfFreq = len(array)

for i in range(2,AmountOfFreq):
    # We start at 2 because on the first line are the column headers and we don't need them.

    helparray = array[i].split()

    helpconv0 = helparray[0]
    helpconv1 = helparray[1]
    inconv = ","
    outconv = "."
    convtable = maketrans(inconv, outconv)
    helparray[0]= helpconv0.translate(convtable)
    helparray[1]= helpconv1.translate(convtable)
    # Audacity exports float with a "," and python read float with a "." so we convert them.
    
    Frequency= float(helparray[0]) 
    List_frequencies.append(float(Frequency))
    # A list is made of all frequencies.

    Decibel= float(helparray[1])
    relativeAmplitude = pow(10,float(Decibel/20))
    List_volumesIn_dB.append(float(relativeAmplitude))
    # Their volume in dB is converted into a relative amplitude and stored in another list.
    
source.close()
    
for samplecount in range(0,int(SampleRate*Time)):

    result = 0

    for i in range(0,AmountOfFreq-2):
        result = result + trunc((32767*(sin(samplecount*((360*List_frequencies[i]*pi)/(180*SampleRate)))))*List_volumesIn_dB[i])
        i += 1

    my_list.append(result)
    # Multiply the absolute sin value with the volumefactor for each frequency

    samplecount += 1




f = open('Hex.wav','wb')
RIFF= 1380533830
f.write(pack('>I', RIFF))
ChunkSize = Subchunk2Size+36
f.write(pack('<I', ChunkSize))
WAVE = 1463899717
f.write(pack('>I', WAVE))


fmt = 1718449184
f.write(pack('>I', fmt))
Subchunk1Size = 16
f.write(pack('<I', Subchunk1Size))
AudioFormat = 1
f.write(pack('<H', AudioFormat))

f.write(pack('<H', NumChannels))
f.write(pack('<I', SampleRate))
f.write(pack('<I', ByteRate))
f.write(pack('<H', BlockAlign))
f.write(pack('<H', BitsPerSample))


data = 1684108385
f.write(pack('>I', data))
f.write(pack('<I', Subchunk2Size))

for item in my_list:
    f.write(pack('<h', item))


f.close()
