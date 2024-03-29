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


### Data   
**[Emoji](https://github.com/YamamotoDesu/OldCollectionView/blob/main/EmojiLibrary/Emoji.swift)**  
```swift
class Emoji {
    enum Category: String, CaseIterable {
        case smileysAndPeople = "Smileys & People"
        case animalsAndNature = "Animals & Nature"
        case foodAndDrink = "Food & Drink"
        case activity = "Activity"
        case travelAndPlaces = "Travel & Places"
        case objects = "Objects"
        case symbols = "Symbols"
        case flags = "Flags"
    }
    
    static let shared = Emoji()
    
    let sections = Category.allCases
    
    var data: [Category: [String]] = [
        .smileysAndPeople: ["😀", "😃", "😄", "😁", "😆", "😅", "😂", "🤣", "☺️", "😊", "🤯", "😇", "🙂", "😎", "🤩", "😴", "😬", "🥵"],
        .animalsAndNature: ["🐶", "🐱", "🦊", "🐻", "🦁", "🐮", "🐸", "🐵", "🐔", "🐧", "🦉", "🐴", "🦋", "🐙", "🐬", "🐈", "🌲", "🌍"],
        .foodAndDrink: ["🍏", "🍇", "🍓", "🥝", "🍅", "🌽", "🥕", "🥨", "🧀", "🍖", "🦴", "🌮", "🍣", "🥤", "🥃", "🥟", "🍺", "🍪"],
        .activity: ["⚽️", "🏉", "🥏", "🏏", "🥅", "🛹", "🛷", "🏋️‍♂️", "🏅", "🎪", "🎬", "🎼", "🎲", "🎳", "🎮", "🎸", "🧩", "🏆"],
        .travelAndPlaces: ["🚗", "🚑", "🚨", "🚠", "🚟", "🚄", "✈️", "🚤", "🚧", "🏠", "⛱", "🌋", "⛩", "🕋", "⛪️", "🌠", "🌇", "🗽"],
        .objects: ["⌚️", "🖱", "🖲", "💾", "☎️", "📺", "💴", "🔨", "🧰", "🧲", "🎁", "🎊", "✉️", "🗳", "📌", "🔍", "🔐", "💰"],
        .symbols: ["❤️", "💔", "☮️", "☯️", "☢️", "🆚", "🉐", "🆘", "❌", "💯", "‼️", "🚸", "⚜️", "♿️", "🔈", "🔔", "♣️", "🚸"],
        .flags: ["🏳️", "🇺🇸", "🇯🇵", "🇩🇪", "🇨🇦", "🇲🇽", "🇧🇷", "🇰🇪", "🇳🇬", "🇮🇳", "🇷🇺", "🇦🇺", "🇫🇷", "🇵🇱", "🇻🇳", "🇱🇹", "🇱🇰", "🇪🇪"]
    ]
    
    private init() {}
    
    static func randomEmoji() -> (Category, String) {
        let extraEmoji = ["💀", "🤖", "👍", "🤘🏾", "🖐🏼", "👇🏽", "🙏🏾", "👀", "👩🏽‍🦱", "👩🏿", "🧕🏽", "🕵🏻‍♂️", "👨🏼‍💻", "👭", "🧚🏾‍♂️", "💍"]
        let randomIndex = Int.random(in: 0..<extraEmoji.count)
        return (.smileysAndPeople, extraEmoji[randomIndex])
    }
    
    func emoji(at indePath: IndexPath) -> String?
    {
        let catagory = sections[indePath.section]
        return data[catagory]?[indePath.item]
    }
}

```

### DataSource   
**[DataSource](https://github.com/YamamotoDesu/OldCollectionView/blob/main/EmojiLibrary/DataSource.swift)**  
```swift
import UIKit

class DataSource: NSObject, UICollectionViewDataSource {
    let emoji = Emoji.shared
    
    func numberOfSections(in collectionView: UICollectionView) -> Int {
        emoji.sections.count
    }
    
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        let catagory = emoji.sections[section]
        let emoji = self.emoji.data[catagory]
        
        return emoji?.count ?? 0
    }
    
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        guard let emojiCell = collectionView.dequeueReusableCell(withReuseIdentifier: EmojiCell.reuseIdentifier, for: indexPath) as? EmojiCell else {
            fatalError("Cell cannot be created")
        }
        
        let catagory = emoji.sections[indexPath.section]
        let emoji = self.emoji.data[catagory]?[indexPath.item] ?? ""
        
        emojiCell.emojiLabel.text = emoji
        
        return emojiCell
    }
    
    func collectionView(_ collectionView: UICollectionView, viewForSupplementaryElementOfKind kind: String, at indexPath: IndexPath) -> UICollectionReusableView {
        guard let emojiHeaderView = collectionView.dequeueReusableSupplementaryView(ofKind: kind, withReuseIdentifier: EmojiHeaderView.reuseIdentifier, for: indexPath) as? EmojiHeaderView else {
            fatalError("Cannot create EmojiHeader")
        }
        
        let catagory = emoji.sections[indexPath.section]
        emojiHeaderView.textLabel.text = catagory.rawValue
        
        return emojiHeaderView
    }
}

extension DataSource {
    func addEmoji(_ emoji: String, to catagory: Emoji.Category) {
        guard var emojiData = self.emoji.data[catagory] else {
            return
        }
        emojiData.append(emoji)
        self.emoji.data.updateValue(emojiData, forKey: catagory)
    }
    
    func deleteEmoji(at indexPath: IndexPath) {
        let catagory = emoji.sections[indexPath.section]
        guard var emojiData = emoji.data[catagory] else {
            return
        }
        emojiData.remove(at: indexPath.item)
        
        emoji.data.updateValue(emojiData, forKey: catagory)
    }
    
    func deleteEmoji(at indexPaths: [IndexPath]) {
        for path in indexPaths {
            deleteEmoji(at: path)
        }
    }
}

```

### UICollectionViewDelegateFlowLayout Delegate  
**[EmojiCollectionViewDelegate](https://github.com/YamamotoDesu/OldCollectionView/blob/main/EmojiLibrary/EmojiCollectionViewDelegate.swift)**  
```swift
import UIKit

class EmojiCollectionViewDelegate: NSObject, UICollectionViewDelegateFlowLayout {
    let numbrOfItemsPerRow: CGFloat
    let interItemSpacing: CGFloat
    
    init(numbrOfItemsPerRow: CGFloat, interItemSpacing: CGFloat) {
        self.numbrOfItemsPerRow = numbrOfItemsPerRow
        self.interItemSpacing = interItemSpacing
    }
    
    // set the size of items
    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize {
        let maxWidth = UIScreen.main.bounds.width
        let totalSpacing = interItemSpacing * numbrOfItemsPerRow

        let itemWidth = (maxWidth - totalSpacing)/numbrOfItemsPerRow
        return CGSize(width: itemWidth, height: itemWidth)
    }

    //  spacing between successive items of a single row
    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, minimumInteritemSpacingForSectionAt section: Int) -> CGFloat {
        return interItemSpacing
    }

    // margin sizes or return different margin sizes for each section
    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, insetForSectionAt section: Int) -> UIEdgeInsets {
        if section == 0 {
            return UIEdgeInsets(top: 0, left: 0, bottom: interItemSpacing/2, right: 0)
        }

        return UIEdgeInsets(top: interItemSpacing/2, left: 0, bottom: interItemSpacing/2, right: 0)
    }
    
}
```
<img src="https://user-images.githubusercontent.com/47273077/130227126-6fac9efa-bc88-4263-a435-096e086010b1.png" width="900" height="500">

### HeaderView   
**[EmojiHeaderView](https://github.com/YamamotoDesu/OldCollectionView/blob/main/EmojiLibrary/EmojiHeaderView.swift)**  
```swift
import UIKit

class EmojiHeaderView: UICollectionReusableView {
    static let reuseIdentifier = String(describing: EmojiHeaderView.self)
    @IBOutlet weak var textLabel: UILabel!
}
```

### Cell   
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

### ViewController   
**[ViewController](https://github.com/YamamotoDesu/OldCollectionView/blob/main/EmojiLibrary/ViewController.swift)**  
```swift
import UIKit

class ViewController: UIViewController {
    
    @IBOutlet weak var collectionView: UICollectionView!
    
    @IBOutlet weak var deleteButton: UIBarButtonItem!
    
    @IBOutlet weak var addButton: UIBarButtonItem!
    
    let dataSource = DataSource()
    let delegate = EmojiCollectionViewDelegate(numbrOfItemsPerRow: 6, interItemSpacing: 8)
    
    override func viewDidLoad() {
        super.viewDidLoad()
        collectionView.dataSource = dataSource
        collectionView.delegate = delegate
        collectionView.allowsMultipleSelection = true
        
        navigationItem.leftBarButtonItem = editButtonItem
    }
    
    override func setEditing(_ editing: Bool, animated: Bool) {
        super.setEditing(editing, animated: animated)
        
        deleteButton.isEnabled = isEditing
        addButton.isEnabled = !isEditing
        
        collectionView.indexPathsForVisibleItems.forEach {
            guard let emojiCell = collectionView.cellForItem(at: $0) as? EmojiCell else {
                return
            }
            emojiCell.isEditing = editing
        }
        
        if isEditing {
            collectionView.indexPathsForSelectedItems?.compactMap({ $0 }).forEach {
                collectionView.deselectItem(at: $0, animated: true)
            }
            return
        }
        
    }
    
    override func shouldPerformSegue(withIdentifier identifier: String, sender: Any?) -> Bool {
        if isEditing && identifier == "showEmojiDetail" {
            return false
        }
        
        return true
    }
    
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        guard  segue.identifier == "showEmojiDetail",
               let emojiCell = sender as? EmojiCell,
               let emojiDetailController = segue.destination as? EmojiDetailController,
               let indexPath = collectionView.indexPath(for: emojiCell),
               let emoji = Emoji.shared.emoji(at: indexPath) else {
            fatalError()
        }
        
        emojiDetailController.emoji = emoji
        
    }
    
    @IBAction func addEmoji(_ sender: Any) {
        
        let (catagory, randomEmoji) = Emoji.randomEmoji()
        dataSource.addEmoji(randomEmoji, to: catagory)
        
        let emojiCount = collectionView.numberOfItems(inSection: 0)
        let insertedIndex = IndexPath(item: emojiCount, section: 0)
        collectionView.insertItems(at: [insertedIndex])
    }
    
    @IBAction func deleteEmoji(_ sender: Any) {
        guard let selectedIndices = collectionView.indexPathsForSelectedItems else {
            return
        }
        let sectionsToDelete = Set(selectedIndices.map({ $0.section }))
        sectionsToDelete.forEach { section in
            let indexPathsForSection = selectedIndices.filter({ $0.section == section})
            let sortedIndexPaths = indexPathsForSection.sorted(by: { $0.item > $1.item })
            dataSource.deleteEmoji(at: sortedIndexPaths)
            collectionView.deleteItems(at: sortedIndexPaths)
        }
        
    }
    
}

```
