@title[Serialise data with boost fusion]

# Serialise data with boost fusion 
Based on the cppnow 2014 talk of [Thomas Rodgers](https://youtu.be/wbZdZKpUVeg?t=1)

---

# Why use boost fusion?

+++

## Flaws of OOD
* OOD needs serialise method
* every data class needs to inherit form Serialiser
* dificult for basic types

```c++

class Serialiser
{
public:
    virtual void serialise() = 0;
};

```

+++

## Flaws of Protobuf etc.
* One needs to control both ends of the communication

+++

# Why not use packed structs ?

* Limited abstraction capabilities
* POD types
* endianness
* compiler dependend specifiers

```c++

struct example
{
    uint16_t value;
    uint16_t timestamp;
    uint32_t crc;
}__attribute__((packed)); //gnu version

```

+++

## Reflection?

Unfortunatly not available in C++

+++

## Boost Fusion to the resque
* 3rd Party systems
* Embedded devices
* Legacy Systems

```c++

BOOST_FUSION_DEFINE_STRUCT
(
(Example), ExampleStruct,
(uint16_t, value)
(uint16_t, timestamp)
(uint32_t, crc)
)

```

+++

## Summary
Use boost fusion when one has control only over one end of the communication.

---

## Boost Fusion

STL containers work on values.

MPL containers work on types

Fusion containers work on both types and values

---

```c++
 
// ==================================================================================================
// ===============================================   Member Functions   =============================
// ==================================================================================================

template<typename T>
class GsiTempPoolEntryT
{
public:
    GsiTempPoolEntryT(){}
    explicit GsiTempPoolEntryT(T val) : m_value(val){}
    T getValue() const { return m_value; }
    virtual unsigned short getId() const = 0;
    virtual SPL_UNIT getUnit() const = 0;
private:
    T m_value;
};
```

```
template<typename T, unsigned short identification, SPL_UNIT unit>
struct GsiEntryT : GsiTempPoolEntryT<T>
{
    GsiEntryT(){}
    GsiEntryT(T val) : GsiTempPoolEntryT<T>(val) {}
    virtual unsigned short getId() const
    {
        return identification;
    }
    virtual SPL_UNIT getUnit() const
    {
        return unit;
    }
};
```
@[1-16](Temperary pool entry) 

```
typedef GsiEntryT<std::string, 11, ASCII> GsiPointIdT;

```
