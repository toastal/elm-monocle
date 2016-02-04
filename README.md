

elm-monocle
===========

[Monocle](http://julien-truffaut.github.io/Monocle/)-inspired library providing purely functional abstractions to manipulate records in [elm](http://www.elm-lang.org/) language.

# Abstractions

## Iso

An Iso is a tool which converts elements of type A into elements of type B and back without loss.

```elm
    type alias Iso a b =
        { get : a -> b
        , reverseGet : b -> a
        }
```

#### Example

```elm
    string2CharListIso : Iso String (List Char)
    string2CharListIso =
        Iso String.toList String.fromList

    (string2CharListIso.get "ABcdE") == ['A','B','c','d','E']
    (string2CharListIso.reverseGet ['A','B','c','d','E']) == "ABcdE"
```

## Prism

A Prism is a tool which optionally converts elements of type A into elements of type B and back.

```elm
    type alias Prism a b =
        { getOption : a -> Maybe b
        , reverseGet : b -> a
        }
```

#### Example

```elm
    string2IntPrism : Prism String Int
    string2IntPrism =
        Prism (String.toInt >> Result.toMaybe) toString

    string2IntPrism.getOption "17896" == Just 17896
    string2IntPrism.getOption "1a896" == Nothing
    string2IntPrism.reverseGet 1626767 = "1626767"
```

## Lens

A Lens is a functional concept which solves a very common problem: how to update a complex immutable structure. Lens acts as a zoom into record.

```elm
    type alias Lens a b =
        { get : a -> b
        , set : b -> a -> a
        }
```

#### Example

```elm
    addressStreetNameLens : Lens Address String
    addressStreetNameLens =
        let
            get a = a.streetName

            set sn a = { a | streetName = sn }
        in
            Lens get set
```