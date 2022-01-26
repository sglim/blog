# Exercism

Learning rust

Learned Vec, vec!, array, list, slice, iter

Sublist problem: https://exercism.org/tracks/rust/exercises/sublist

My solution: https://exercism.org/tracks/rust/exercises/sublist/iterations

```Rust
#[derive(Debug, PartialEq)]
pub enum Comparison {
    Equal,
    Sublist,
    Superlist,
    Unequal,
}
pub fn sublist<T: PartialEq>(first_list: &[T], second_list: &[T]) -> Comparison {
    // 앞 뒤 것을 빼 보고... 같으면...
    if first_list == second_list {
        return Comparison::Equal;
    }
    if first_list.is_empty() || second_list.is_empty() {
        return Comparison::Unequal;
    }
    if first_list.len() == second_list.len() {
        return Comparison::Unequal;
    }
    if first_list.len() > second_list.len() {
        // 앞에서 하나 지워보고 뒤에서 하나 지워봄
        if sublist(&first_list[1..], second_list) != Comparison::Unequal ||
                sublist(&first_list[..first_list.len() - 1], second_list) != Comparison::Unequal {
            return Comparison::Superlist
        }
    }
    if first_list.len() < second_list.len() {
        // 앞에서 하나 지워보고 뒤에서 하나 지워봄
        if sublist(first_list, &second_list[1..]) != Comparison::Unequal ||
                sublist(first_list, &second_list[..second_list.len() - 1]) != Comparison::Unequal {
            return Comparison::Sublist
        }
    }
    
    Comparison::Unequal
}
```

https://exercism.org/tracks/rust/exercises/sublist/solutions/alireza4050

```Rust
#[derive(Debug, PartialEq)]
pub enum Comparison {
    Equal,
    Unequal,
    Sublist,
    Superlist,
}
pub fn sublist<T: Eq>(a: &[T], b: &[T]) -> Comparison {
    use Comparison::*;
    match (a.len(), b.len()) {
        (0, 0) => Equal,
        (0, _) => Sublist,
        (_, 0) => Superlist,
        (m, n) if m > n => if a.windows(n).any(|v| v == b) {Superlist} else {Unequal},
        (m, n) if m < n => if b.windows(m).any(|v| v == a) {Sublist} else {Unequal},
        (_, _) => if a == b {Equal} else {Unequal},
    }
}
```