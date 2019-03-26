%tile: Serialise data with boost fusion
%author: THUC, 3330, @ThurnheerC
%date 2019-02-12

# Serialise data with boost fusion 

---

# Why not use OOD or Protobufs etc.?

## Flaws of OOD
* OOD needs serialise method
* every data class needs to inherit form Serialiser
* dificult for basic types

## Flaws of Protobuf etc.
* One needs to control both ends of the communication

## Boost Fusion to the resque
* 3rd Party systems
* Embedded devices
* Legacy Systems

---

# Why not use packed structs ?

* Limited abstraction capabilities
** POD types
** endianness

---

## Reflection?

Unfortunatly not available in C++

---

## Boost Fusion

STL containers work on values.

MPL containers work on types

Fusion containers work on both types and values

---

```
 
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

typedef GsiEntryT<std::string, 11, ASCII> GsiPointIdT;

```
