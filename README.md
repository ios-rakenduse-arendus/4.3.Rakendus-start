
# 4.3. Rakendus: START e. liikumise lisamine


### 1. Paneme eksmati liikuma, kui ekraanile vajutatakse. Selleks ava *GameScene.swift* fail ning otsi sealt üles funktsioon nimega ```touchesBegan``` ja kirjutame sinna järgmise koodi:

```swift
    if isGameStarted == false{
        isGameStarted =  true
        eksmati.physicsBody?.affectedByGravity = true
        createPauseBtn()
        
        logoImg.run(SKAction.scale(to: 0.5, duration: 0.3), completion: {
            self.logoImg.removeFromParent()
        })
        taptoplayLbl.removeFromParent()

       
        eksmati.physicsBody?.velocity = CGVector(dx: 0, dy: 0)
        eksmati.physicsBody?.applyImpulse(CGVector(dx: 0, dy: 40))
    } else {
    
 
        if isDied == false {
            eksmati.physicsBody?.velocity = CGVector(dx: 0, dy: 0)
            eksmati.physicsBody?.applyImpulse(CGVector(dx: 0, dy: 40))
        }
    }
```   
---   

**Mängi rakendust nüüd simulatoris ning kontrolli, kas ekraani vajutusel logo ja "Tap to play" kaovad eest ja Eksmati hakkab hüplema. Seniks kuni ekraani vajutada peaks Eksmati õhus püsima ning kui ekraanile vajutamine lõpetada kukub Eksmati maha.**

---

**VIDEO(1.):**

<a href="https://youtu.be/zuuT1pb8cfA
" target="_blank"><img src="http://img.youtube.com/vi/zuuT1pb8cfA/0.jpg" 
alt="Projekti loomine" width="240" height="180" border="10" /></a>

# ÜLESANNE:
* Täida järgnevas 2. puntis olevas koodis kommentaaride all **LISA SIIA** kohtades vajalikud koodiread alumise posti loomiseks.
---

### 2. Loome ekraanile postid ja EAP'd. Selleks avame *GameElements.swift* faili ning kirjutame sinna *GameScene*'i järgmise koodi:

``` swift
func createWalls() -> SKNode  {

        let eapNode = SKSpriteNode(imageNamed: "EAP")
        eapNode.size = CGSize(width: 40, height: 40)
        eapNode.position = CGPoint(x: self.frame.width + 25, y: self.frame.height / 2)
        eapNode.physicsBody = SKPhysicsBody(rectangleOf: eapNode.size)
        eapNode.physicsBody?.affectedByGravity = false
        eapNode.physicsBody?.isDynamic = false
        eapNode.physicsBody?.categoryBitMask = CollisionBitMask.EAPCategory
        eapNode.physicsBody?.collisionBitMask = 0
        eapNode.physicsBody?.contactTestBitMask = CollisionBitMask.eksmatiCategory
        eapNode.color = SKColor.blue
        


        wallPair = SKNode()
        wallPair.name = "wallPair"
        
        let topWall = SKSpriteNode(imageNamed: "post")
        // LISA SIIA btmWALL sama pildi nimega
        
        topWall.position = CGPoint(x: self.frame.width + 25, y: self.frame.height / 2 + 420)
        // LISA SIIA btmWall mille x väärtus on sama kuid y väärtusele mitte ei liideta vaid lahutatakse 420
        
        
        topWall.setScale(0.5)
        // LISA SIIA btmWall sama Scale väärtusega
        
        topWall.physicsBody = SKPhysicsBody(rectangleOf: topWall.size)
        topWall.physicsBody?.categoryBitMask = CollisionBitMask.postCategory
        topWall.physicsBody?.collisionBitMask = CollisionBitMask.eksmatiCategory
        topWall.physicsBody?.contactTestBitMask = CollisionBitMask.eksmatiCategory
        topWall.physicsBody?.isDynamic = false
        topWall.physicsBody?.affectedByGravity = false
        // LISA SIIA järjest kõik read samamoodi btmWall jaoks(kokkupuuted posti
        // ja eksmati kategooriaga ning gravitatsiooni ja liikumise piiramine)
        
        
        topWall.zRotation = CGFloat.pi
        
        wallPair.addChild(topWall)
        // LISA SIIA samaäärne kood btmWall'i kohta
        
        wallPair.zPosition = 1
        

        let randomPosition = random(min: -200, max: 200)
        wallPair.position.y = wallPair.position.y +  randomPosition
        wallPair.addChild(eapNode)
        
        

        wallPair.run(moveAndRemove)
        
        return wallPair
}
func random() -> CGFloat{
        return CGFloat(Float(arc4random()) / 0xFFFFFFFF)
}
func random(min : CGFloat, max : CGFloat) -> CGFloat{
        return random() * (max - min) + min
}
``` 

### 4. Kuvame ekraanile eelnevalt loodud postid. Ava *GameScene.swift* fail ja kirjutame seal ennem üles märgitud **TODO:** asemele  järgmise koodi:

```swift 
let spawn = SKAction.run({
    () in
    self.wallPair = self.createWalls()
    self.addChild(self.wallPair)
})


let delay = SKAction.wait(forDuration: 1.5)
let SpawnDelay = SKAction.sequence([spawn, delay])
let spawnDelayForever = SKAction.repeatForever(SpawnDelay)
self.run(spawnDelayForever)


let distance = CGFloat(self.frame.width + wallPair.frame.width)
let movePostid = SKAction.moveBy(x: -distance - 50, y: 0, duration: TimeInterval(0.008 * distance))
let removePostid = SKAction.removeFromParent()
moveAndRemove = SKAction.sequence([movePostid, removePostid])
``` 
---   

**Käivita rakendus simulatoris ning alusta mängu. Nüüd sa peaksid nägema paremalt ekraanile ilmuvaid poste mille vahel on EAP'd.** 

---

**VIDEO(2.,3.,4.):**

<a href="https://youtu.be/_z-FwQuJikQ
" target="_blank"><img src="http://img.youtube.com/vi/_z-FwQuJikQ/0.jpg" 
alt="Projekti loomine" width="240" height="180" border="10" /></a>

>Eksmati ei kogu hetkel EAP'sid ning kui ta puutub kokku seinaga siis tiritakse ta ekraanist välja. 
>
>Järgmises tunnis lisame funktsiooni, mis lõpetab mängu siis, kui Eksmati puutub kokku seinaga ning kui Eksmati puudutab EAP'd siis suureneb ekraanil kuvatud EAP'de nuber ja ekraanilt kaob ära EAP millega just kokku puututi.

### Teadmiste kontroll
* Täida ülesanne *[Learningapps](https://learningapps.org/watch?v=pg7wvin2v18)* lehel. 
