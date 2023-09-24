# Graphics and textures

## Character intros
### Useful notes
|||
|:---:|:---:|
|Text font|VoxBox|
|Text rotation|12,97°|
|Font size|About 109 px|

### Avoiding cut text
Character intro animations present a very annoying issues to all aspiring translators, as the text gets cut if you attempt to put the text (or part of it) beyond the upmost, lowest, leftmost and rightmost pixel of the original text. In order to fix this, it is necessary to edit the `textureRect` and `textureRectOffset` under the `m_RD` property of the respective `Sprite`.
