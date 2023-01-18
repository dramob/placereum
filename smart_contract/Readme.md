pour l'implémentation que l'on cherche à faire , on peut tout d'abord verifier que le pixel dont on veut changer la couleur appartien bien au bon owner ;voici une ebauche de solution ;
pragma solidity ^0.6.0;

// Pixel NFT Contract
contract Pixel {
    // Variable to store the pixel color
    uint256 pixelColor;

    // The owner of the pixel
    address owner;

    // The constructor of the contract
    constructor(uint256 _pixelColor) public {
        owner = msg.sender;
        pixelColor = _pixelColor;
    }

    // Function to change the color of the pixel
    function changePixelColor(uint256 _newPixelColor) public {
        // Only the owner of the pixel can change its color
        require(msg.sender == owner, "Only the owner can change the color of the pixel");
        pixelColor = _newPixelColor;
    }
}


Concernant la transaction , si l'on fait pour l'instant abstraction du prix modulable , voici l'architecture que devrait avoir la séquence;


contract PixelPurchase {
    address public owner;
    uint256 public price;

    constructor(uint256 _price) public {
        owner = msg.sender;
        price = _price;
    }

    function purchase() public payable {
        require(msg.value == price);
        owner = msg.sender;
    }

    function refund() public {
        require(msg.sender == owner);
        msg.sender.transfer(this.balance);
    }

}
