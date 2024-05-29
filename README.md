# redux-react-i18n

## Example Demo

```
git clone https://github.com/derzunov/redux-react-i18n redux-react-i18n
cd redux-react-i18n/example
npm i
```
and then
```
gulp
```
or
```
gulp prod
```

## Short code demo

#### Write ( jsx ):
```jsx
<Loc locKey="your_key_1"/>
<Loc locKey="your_key_2" number={1}/>
<Loc locKey="your_key_2" number={2}/>
<Loc locKey="your_key_2" number={5}/>
```
#### Result ( html ):
```html
<span>Перевод вашего первого ключа из словаря для текущего языка</span>
<span>Пришла 1 кошечка</span>
<span>Пришли 2 кошечки</span>
<span>Пришло 5 кошечек</span>
```

## Install:
Terminal:
```
npm i redux-react-i18n
```

## What's in the box

### Components:
 - Loc ( Container Component )
 - LocPresentational ( Presentational Component )

### Actions
 - setCurrentLanguage( languageCode )
 - setLanguages( languageCode )
 - addDictionary( languageCode, dictionary )
 - setDictionaries( dictionaries )

### Reducer
 - i18n


## Full code demo ( complete solution for Redux ):
```jsx
import { i18nReducer, i18nActions, Loc } from 'redux-react-i18n'

...
// "reducers" contains your reducers
reducers.i18n = i18nReducer
...

const store = createStore( combineReducers( reducers ) )

...
// Set dictionaries (simpliest example) -----------------------------------------------------------------------------------------------

// This dictionaries can be supported by Localization team without need to know somth about interface or project,
// and you just can fetch it to your project
const dictionaries = {
    'ru-RU': {
        'key_1': 'Первый дефолтный ключ',
        'key_2': [ '$Count', ' ', ['штучка','штучки','штучек']], // 1 штучка, 3 штучки, пять штучек
        'key_3': {
            'nested_1': 'Первый вложенный ключ',
            'nested_2': 'Второй вложенный ключ',
        },
        /* ... */
        /* Other keys */
    },
    'en-US': {
        'key_1': 'First default key',
        'key_2': [ '$Count', ' ', ['thing','things']], // 1 thing, 2 things, 153 things
        'key_3': {
            'nested_1': 'First nested key',
            'nested_2': 'Second nested key',
        },
    }
    /* ... */
    /* Other dictionaries */
}
store.dispatch( i18nActions.setDictionaries( dictionaries ) )
// / Set dictionaries (simpliest example) ---------------------------------------------------------------------------------------------

// Set languages (simpliest example) --------------------------------------------------------------------------------------------------
const languages = [
    {
        code: 'ru-RU',
        name: 'Русский'
    },
    {
        code: 'en-US',
        name: 'English (USA)'
    }
    /* ... */
    /* Other languages */
]

store.dispatch( i18nActions.setLanguages( languages ) )
// / Set languages (simpliest example) ------------------------------------------------------------------------------------------------

// Set current language code (you can map this action to select component or somth like this)
store.dispatch( i18nActions.setCurrentLanguage( 'ru-RU' ) )
```

#### And now you can use "Loc" container component

```jsx
import { Loc } from 'redux-react-i18n'
...

<p>
  <Loc locKey="key_1"/> // => Первый дефолтный ключ
</p>

<p>
  <Loc locKey="key_2" number={7}/> // => 7 штучек
</p>

<p>
  <Loc locKey="key_3.nested_1"/> // => Первый вложенный ключ
</p>
<p>
  <Loc locKey="key_3.nested_2"/> // => Второй вложенный ключ
</p>
```

## If you don't want to use a complete solution:

#### Just use a dumb component and you can design store/actions/reducers by yourself like you want

```jsx
// Just import presentational component LocPresentational
import { LocPresentational } from 'redux-react-i18n'
...
...
...
// Then map data to props => currentLanguage, dictionary (See more in src/Loc.js):
const mapStateToProps = ( { /*getting data from the state*/ }, ownProps ) => ({
    currentLanguage: yourCurrentLanguageFromState,
    dictionary: yourDictionaryFromState
});
Loc = connect( mapStateToProps )( LocPresentational )
...
...
...
<p>
  <Loc locKey="YOUR_KEY_1"/>
</p>

<p>
  <Loc locKey="YOUR_KEY_2"  number={42}/>
</p>
```
See more in src/\*.js


## The "Span Problem"

If the span tag is a big problem (in "option" tag for example), you can use **translate** from 'translatr' like this

```
import translate from 'translatr'
...
...
...
<select>
   <option value="1">
      { translate( dictionary, currentLanguage, key, number ) }
   </option>
</select>
...
```

and just a simple example of **mapStateToProps** as a bonus:

```
const mapStateToProps = ( {i18n: { currentLanguage, dictionaries }}, ownProps ) => ({
    currentLanguage: currentLanguage,
    dictionary: dictionaries[ currentLanguage ]
});
```

#### Why?

With ```<Loc locKey="your_key" ></Loc>``` you'll get:

```
<select>
  <option> <span>Translated Text</span> </option>
</select>
```
