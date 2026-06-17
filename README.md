import random
import time


def interpolation_search(arr, target):
    """
    Interpolation Search Algorithm
    Time Complexity: O(log log n) average, O(n) worst case
    Space Complexity: O(1)
    """
    low, high = 0, len(arr) - 1
    comparisons = 0
    while low <= high and arr[low] <= target <= arr[high]:
        comparisons += 1
        if low == high:
            if arr[low] == target:
                return low, comparisons
            return -1, comparisons
        pos = low + int(((target - arr[low]) * (high - low))
                         / (arr[high] - arr[low]))
        if arr[pos] == target:
            return pos, comparisons
        elif arr[pos] < target:
            low = pos + 1
        else:
            high = pos - 1
    return -1, comparisons


def binary_search(arr, target):
    """Binary Search for comparison"""
    low, high = 0, len(arr) - 1
    comparisons = 0
    while low <= high:
        comparisons += 1
        mid = (low + high) // 2
        if arr[mid] == target:
            return mid, comparisons
        elif arr[mid] < target:
            low = mid + 1
        else:
            high = mid - 1
    return -1, comparisons


def get_user_array():
    """Prompt the user for a sorted array, with basic validation."""
    while True:
        raw = input("Enter sorted numbers separated by commas (e.g. 2,5,10,15,23): ").strip()
        if not raw:
            print("Input cannot be empty. Try again.\n")
            continue
        try:
            arr = [int(x.strip()) for x in raw.split(",")]
        except ValueError:
            print("Invalid input. Please enter integers separated by commas.\n")
            continue
        if arr != sorted(arr):
            choice = input("Array is not sorted. Sort it automatically? (y/n): ").strip().lower()
            if choice == "y":
                arr = sorted(arr)
            else:
                print("Interpolation/Binary search require a sorted array. Try again.\n")
                continue
        if len(arr) == 0:
            print("Array must contain at least one element.\n")
            continue
        return arr


def get_user_target():
    """Prompt the user for the target value to search for."""
    while True:
        raw = input("Enter the target value to search for: ").strip()
        try:
            return int(raw)
        except ValueError:
            print("Invalid input. Please enter an integer.\n")


def performance_analysis():
    sizes = [1000, 5000, 10000, 50000, 100000]
    print(f"{'Size':>10} {'IS Time(ms)':>14} {'BS Time(ms)':>14} "
          f"{'IS Comparisons':>16} {'BS Comparisons':>16}")
    print('-' * 75)
    for size in sizes:
        arr = sorted(random.sample(range(size * 10), size))
        target = arr[random.randint(0, size - 1)]
        start = time.perf_counter()
        for _ in range(100):
            idx_is, comp_is = interpolation_search(arr, target)
        is_time = (time.perf_counter() - start) / 100 * 1000
        start = time.perf_counter()
        for _ in range(100):
            idx_bs, comp_bs = binary_search(arr, target)
        bs_time = (time.perf_counter() - start) / 100 * 1000
        print(f"{size:>10} {is_time:>14.4f} {bs_time:>14.4f} "
              f"{comp_is:>16} {comp_bs:>16}")


if __name__ == "__main__":
    arr = get_user_array()
    target = get_user_target()
    idx_is, comp_is = interpolation_search(arr, target)
    idx_bs, comp_bs = binary_search(arr, target)
    print()
    print(f"Array: {arr}")
    print(f"Searching for: {target}")
    print(f"Interpolation Search -> Found at index: {idx_is}, Comparisons: {comp_is}")
    print(f"Binary Search        -> Found at index: {idx_bs}, Comparisons: {comp_bs}")
    print()
    run_perf = input("Run the performance analysis on random large arrays too? (y/n): ").strip().lower()
    if run_perf == "y":
        print()
        performance_analysis()
