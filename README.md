import json
import os
from datetime import date

FILE = "habits.json"

def load_habits():
    if os.path.exists(FILE):
        with open(FILE, "r", encoding="utf-8") as :
            return json.load(f)
    return []

def save_habits(habits):
    with open(FILE, "w", encoding="utf-8") as f:
        json.dump(habits, f, indent=4, ensure_ascii=False)

def add_habit(habits):
    name = input("Habit name: ").strip()

    if not name:
        print("Habit name cannot be empty.")
        return

    habits.append({
        "name": name,
        "completed_dates": []
    })

    save_habits(habits)
    print("Habit added successfully!")

def show_habits(habits):
    if not habits:
        print("No habits found.")
        return

    today = date.today().isoformat()

    for i, habit in enumerate(habits, 1):
        status = "Done" if today in habit["completed_dates"] else "Not done"
        print(f"{i}. {habit['name']} - {status}")

def complete_habit(habits):
    show_habits(habits)

    if not habits:
        return

    try:
        number = int(input("Habit number completed: "))

        if 1 <= number <= len(habits):
            today = date.today().isoformat()
            completed = habits[number - 1]["completed_dates"]

            if today not in completed:
                completed.append(today)
                save_habits(habits)
                print("Great! Habit completed.")
            else:
                print("Already completed today.")
        else:
            print("Invalid habit number.")

    except ValueError:
        print("Please enter a valid number.")

def show_progress(habits):
    if not habits:
        print("No habits found.")
        return

    today = date.today().isoformat()
    completed = sum(
        today in habit["completed_dates"]
        for habit in habits
    )

    total = len(habits)
    percentage = (completed / total) * 100

    print(f"Today's progress: {completed}/{total}")
    print(f"Completion rate: {percentage:.1f}%")

def delete_habit(habits):
    show_habits(habits)

    if not habits:
        return

    try:
        number = int(input("Habit number to delete: "))

        if 1 <= number <= len(habits):
            removed = habits.pop(number - 1)
            save_habits(habits)
            print(f"Deleted: {removed['name']}")
        else:
            print("Invalid habit number.")

    except ValueError:
        print("Please enter a valid number.")

def main():
    habits = load_habits()

    while True:
        print("\n===== HABIT TRACKER =====")
        print("1. Add habit")
        print("2. Show habits")
        print("3. Complete habit")
        print("4. View progress")
        print("5. Delete habit")
        print("6. Exit")

        choice = input("Choose an option: ")

        if choice == "1":
            add_habit(habits)
        elif choice == "2":
            show_habits(habits)
        elif choice == "3":
            complete_habit(habits)
        elif choice == "4":
            show_progress(habits)
        elif choice == "5":
            delete_habit(habits)
        elif choice == "6":
            print("Goodbye!")
            break
        else:
            print("Invalid choice. Try again.")

if __name__ == "__main__":
    main()
