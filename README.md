[9:54 am, 02/06/2023] Shrikant 😊: #include <SPI.h>

#include <MFRC522.h>

const uint8_t RST PIN D3;

const uint8_t SS PIN-D4;

MFRC522 mfre522(SS_PIN, RST PIN);
MFRC522::MIFARE_Key key;

int blockNum-4;

byte bufferLen = 18;

byte readBlockData[18];

MFRC522::StatusCode status;

void setup() {

Serial.begin(9600);

SPL.begin();

mfrc522.PCD_Init();
Serial.println("Scan the RFID TAG to store the data :)");}

void loop(){
for (byte i=0; i<6; i++){

key.keyByte[i] = 0xFF; }

if (!mfre522.PICC_IsNewCardPresent()){return;}
if(!mfrc522.PICC_ReadCardSerial()) {return;}

Serial.print("\n");

Serial.println("*Card Detected*");

Serial.print(F("Card UID:"));
for (byte i=0; i<mfrc522.uid.size; i++){

Serial.print(mfrc522.uid.uidByte[i] <0x10?"0":"");

Serial.prin…
[9:54 am, 02/06/2023] Shrikant 😊: void WriteDataToBlock(int blockNum, byte blockData[]) { mfre522.PCD Authenticate(MFRCS22::PICC_CMD_MF_AUTH

status=

KEY_A, blockNum, &key, &(mfre522.uid)); if (status != MFRC522::STATUS_OK){

Serial.print("Authentication failed for Write: ");

Serial.println(mfre522.GetStatusCodeName(status));

return; } else { }

status = mfre522.MIFARE_Write(blockNum, blockData, 16);

if (status != MFRC522::STATUS_OK) {

Serial.print("Writing to Block failed: ");

Serial.println(mfrc522.GetStatusCodeName(status));

return; } else { }}

void ReadDataFromBlock(int blockNum, byte readBlockData[]) {

for (byte i=0; i<6; i++) {

key.keyByte[i] = 0xFF; }

status=

mfrc522.PCD_Authenticate(MFRC522::PICC_CMD_MF_AUTH_ KEY_A, blockNum, &key, &(mfrc522.uid));

if (status != MFRC522::STATUS_OK){

Serial.print("Authentication failed for Read: ");

Serial.println(mfrc522.GetStatusCodeName(status));

return; }

else {}

status = mfrc522.MIFARE_Read(blockNum, readBlockData,

&bufferLen);

if (status != MFRC522::STATUS_OK) {

Serial.print("Reading failed: ");

Serial.println(mfre522.GetStatusCodeName(status));

return; }

else { }}

void dumpSerial(int blockNum, byte blockData[]) { Serial.print("\n");

Serial.print("Data saved on block"); Serial.print(blockNum);

Serial.print(":");

for (int j=0; j<16; j++){

Serial.write(readBlockData[j]); } Serial.print("\n");

for(int i = 0; i < sizeof(readBlockData); ++i) readBlockData[i]= (char)0; //empty space}
