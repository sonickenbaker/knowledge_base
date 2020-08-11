# Example scripts

```python
import argparse

def parse_command_line():
    parser = argparse.ArgumentParser(
        description="Commit deploy stuff on salt_automation")
    parser.add_argument("-s", "--sprint", dest="sprint",
                        help="sprint to release", required=True)
    parser.add_argument("--exclude-components", dest="components_to_be_excluded", default=[],
                        help="components to be excluded from release", nargs="*", required=False)
    parser.add_argument("-e", "--environments", dest="env_list",
                        help="list of environments", default=["sandbox", "staging", "production"],
                        nargs="*", required=False)
    parser.add_argument("--manual-list", dest="manual_list",
                        help="list of components and their versions <component>:<version>",
                        default=[], nargs="*", required=False)
    parser.add_argument("--status", dest="status",
                        help="issues status", default=DEFAULT_STATUS, required=False)
    return parser.parse_args()

if __name__ == '__main__':
    main()
```