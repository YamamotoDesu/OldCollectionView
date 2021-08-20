# OldCollectionView  
## **[Context](https://developer.apple.com/documentation/uikit/uicollectionview)**  
### Implementing Old Collection Views
An object that manages an ordered collection of data items and presents them using customizable layouts.
When you add a collection view to your user interface, your app's main job is to manage the data associated with that collection view. The collection view gets its data from the data source object, stored in the collection view’s dataSource property. 
For your data source, you can create a custom data source object by adopting the UICollectionViewDataSource protocol.

## Preview  
<img src="https://user-images.githubusercontent.com/47273077/130161006-13d22ace-1b5b-4693-a0d2-c20b46c65973.png" width="300" height="600">

## Sample Code  
### Storyboard  
<img src="https://user-images.githubusercontent.com/47273077/130161412-589ca8c6-ac8b-41b4-bfd1-a134b8d12450.png" width="600" height="300">  

### HeaderView   
**[EmojiHeaderView](https://github.com/YamamotoDesu/OldCollectionView/blob/main/EmojiLibrary/EmojiHeaderView.swift)**  
```swift
import UIKit

class EmojiHeaderView: UICollectionReusableView {
    static let reuseIdentifier = String(describing: EmojiHeaderView.self)
    @IBOutlet weak var textLabel: UILabel!
}
```

### EmojiCell   
**[EmojiHeaderView](https://github.com/YamamotoDesu/OldCollectionView/blob/main/EmojiLibrary/EmojiCell.swift)**  
```swift
import UIKit

class EmojiCell: UICollectionViewCell {
    static let reuseIdentifier = String(describing: EmojiCell.self)
    
    @IBOutlet weak var emojiLabel: UILabel!
    
    var isEditing: Bool = false
    
    override var isSelected: Bool {
        didSet {
            if isEditing {
                contentView.backgroundColor = isSelected ? UIColor.systemRed.withAlphaComponent(0.5) : UIColor.systemGroupedBackground
            } else {
                contentView.backgroundColor = UIColor.systemGroupedBackground
            }
        }
    }
}

```
