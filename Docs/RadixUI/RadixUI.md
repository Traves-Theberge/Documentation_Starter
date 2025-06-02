TITLE: Applying Positioning Props to Box Component in JSX
DESCRIPTION: Demonstrates the use of positioning props on the Box component, including responsive positioning and inset values.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/overview/layout.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Box position="relative" />
<Box position={{ initial: "relative", lg: "sticky" }} />

<Box inset="4" />
<Box inset={{ initial: "0", xl: "auto" }} />

<Box left="4" />
<Box left={{ initial: "0", xl: "auto" }} />
```

----------------------------------------

TITLE: Using Box Component with Padding Props in JSX
DESCRIPTION: Demonstrates how to use the Box component with various padding prop configurations, including responsive object values.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/overview/layout.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Box p="4" />
<Box p="100px">
<Box p={{ sm: '6', lg: '9' }}>
```

----------------------------------------

TITLE: Composing Text with Checkbox Form Control in Radix UI (JSX)
DESCRIPTION: This example demonstrates how to compose a Text component with a Checkbox form control. The Text component is used as a label that wraps the Checkbox, creating a properly aligned multi-line form control. The Flex component with a gap property helps maintain proper spacing between the checkbox and text content.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/text.mdx#2025-04-21_snippet_17

LANGUAGE: jsx
CODE:
```
<Box maxWidth="300px">
	<Text as="label" size="3">
		<Flex gap="2">
			<Checkbox defaultChecked /> I understand that these documents are
			confidential and cannot be shared with a third party.
		</Flex>
	</Text>
</Box>
```

----------------------------------------

TITLE: Basic Theme Configuration in React
DESCRIPTION: This example demonstrates wrapping a component tree in the Theme component to provide or modify configuration for all children. It includes various UI components like Card, TextArea, Switch, and Button.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/theme.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Box maxWidth="400px">
	<Card size="2">
		<Flex direction="column" gap="3">
			<Grid gap="1">
				<Text as="div" weight="bold" size="2" mb="1">
					Feedback
				</Text>
				<TextArea placeholder="Write your feedback…" />
			</Grid>
			<Flex asChild justify="between">
				<label>
					<Text color="gray" size="2">
						Attach screenshot?
					</Text>
					<Switch size="1" defaultChecked />
				</label>
			</Flex>
			<Grid columns="2" gap="2">
				<Button variant="surface">Back</Button>
				<Button>Send</Button>
			</Grid>
		</Flex>
	</Card>
</Box>
```

----------------------------------------

TITLE: Labeling with Flex and Checkbox in JSX
DESCRIPTION: This snippet illustrates how to label a Checkbox using Text and Flex components in JSX. It demonstrates creating a labeled Checkbox that is default checked to agree to terms. This implementation requires the Checkbox, Text, and Flex components to be imported. The components are used to create a simple form-like component.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/checkbox.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Text as="label" size="2">
	<Flex gap="2">
		<Checkbox defaultChecked />
		Agree to Terms and Conditions
	</Flex>
</Text>
```

----------------------------------------

TITLE: Using Visually Hidden with an Icon
DESCRIPTION: This example demonstrates how to use the `VisuallyHidden.Root` component to provide accessible text for an icon. The `GearIcon` component is displayed visually, while the "Settings" text is hidden visually but remains accessible to screen readers.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/utilities/visually-hidden.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
```jsx
import { VisuallyHidden } from "radix-ui";
import { GearIcon } from "@radix-ui/react-icons";

export default () => (
	<button>
		<GearIcon />
		<VisuallyHidden.Root>Settings</VisuallyHidden.Root>
	</button>
);
```
```

----------------------------------------

TITLE: Implementing Abstracted Select Component in React
DESCRIPTION: Complete implementation of a custom abstracted Select component that wraps Radix UI's primitive Select parts into a more user-friendly API with predefined structure and styling, including icon integration.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/select.mdx#2025-04-21_snippet_13

LANGUAGE: jsx
CODE:
```
// your-select.jsx
import * as React from "react";
import { Select as SelectPrimitive } from "radix-ui";
import {
	CheckIcon,
	ChevronDownIcon,
	ChevronUpIcon,
} from "@radix-ui/react-icons";

export const Select = React.forwardRef(
	({ children, ...props }, forwardedRef) => {
		return (
			<SelectPrimitive.Root {...props}>
				<SelectPrimitive.Trigger ref={forwardedRef}>
					<SelectPrimitive.Value />
					<SelectPrimitive.Icon>
						<ChevronDownIcon />
					</SelectPrimitive.Icon>
				</SelectPrimitive.Trigger>
				<SelectPrimitive.Portal>
					<SelectPrimitive.Content>
						<SelectPrimitive.ScrollUpButton>
							<ChevronUpIcon />
						</SelectPrimitive.ScrollUpButton>
						<SelectPrimitive.Viewport>{children}</SelectPrimitive.Viewport>
						<SelectPrimitive.ScrollDownButton>
							<ChevronDownIcon />
						</SelectPrimitive.ScrollDownButton>
					</SelectPrimitive.Content>
				</SelectPrimitive.Portal>
			</SelectPrimitive.Root>
		);
	},
);

export const SelectItem = React.forwardRef(
	({ children, ...props }, forwardedRef) => {
		return (
			<SelectPrimitive.Item {...props} ref={forwardedRef}>
				<SelectPrimitive.ItemText>{children}</SelectPrimitive.ItemText>
				<SelectPrimitive.ItemIndicator>
					<CheckIcon />
				</SelectPrimitive.ItemIndicator>
			</SelectPrimitive.Item>
		);
	},
);
```

----------------------------------------

TITLE: Creating Ghost Buttons in JSX
DESCRIPTION: This snippet demonstrates the use of the 'ghost' variant to create a button without chrome, using JSX. A ghost button behaves like text in layout, utilizing a negative margin for alignment. No additional dependencies are required, and typical usage is styling.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/button.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Button variant="ghost">Edit profile</Button>
```

----------------------------------------

TITLE: Using Popover with Content and Trigger
DESCRIPTION: This JSX snippet demonstrates the basic structure of a Popover component using Radix UI, including Popover.Root, Popover.Trigger, Popover.Portal and Popover.Content components. It also sets a custom class name and side offset for the content.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/popover.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
"// index.jsx
import { Popover } from \"radix-ui\";
import \"./styles.css\";

export default () => (
	<Popover.Root>
		<Popover.Trigger>…</Popover.Trigger>
		<Popover.Portal>
			<Popover.Content __className__=\"PopoverContent\" sideOffset={5}>
				…
			</Popover.Content>
		</Popover.Portal>
	</Popover.Root>
);"
```

----------------------------------------

TITLE: Close Icon Button for Toasts in React
DESCRIPTION: This snippet demonstrates how to label the close icon button in a Toast for screen reader users. The aria-label attribute provides a text description for the button, and the aria-hidden attribute hides the icon from screen readers, preventing redundant announcements.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/toast.mdx#2025-04-21_snippet_9

LANGUAGE: jsx
CODE:
```
<Toast.Root type="foreground">
	<Toast.Description>Saved!</Toast.Description>
	<Toast.Close aria-label="Close">
		<span aria-hidden>×</span>
	</Toast.Close>
</Toast.Root>
```

----------------------------------------

TITLE: Spreading Props in Custom React Component for Radix UI
DESCRIPTION: The snippet demonstrates the importance of spreading props in a custom React component to ensure compatibility with Radix UI's components. It shows how to use JSX's spread operator to pass Radix's props and event handlers to the underlying DOM element, making the component functional and accessible.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/guides/composition.mdx#2025-04-21_snippet_1

LANGUAGE: JavaScript
CODE:
```
// before
const MyButton = () => <button />;

// after
const MyButton = (props) => <button {...props} />;
```

----------------------------------------

TITLE: Creating Custom Button with asChild Prop
DESCRIPTION: Example of implementing a flexible button component using Slot for dynamic rendering based on asChild prop
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/utilities/slot.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
import * as React from "react";
import { Slot } from "radix-ui";

function Button({ asChild, ...props }) {
	const Comp = asChild ? Slot.Root : "button";
	return <Comp {...props} />;
}
```

----------------------------------------

TITLE: Button Usage with asChild Prop
DESCRIPTION: Example of using a custom button component with asChild to render as an anchor tag
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/utilities/slot.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
import { Button } from "./your-button";

export default () => (
	<Button asChild>
		<a href="/contact">Contact</a>
	</Button>
);
```

----------------------------------------

TITLE: Integrating Radix UI Themes with next-themes
DESCRIPTION: Implementation example showing how to integrate Radix UI Themes with next-themes library to support system appearance preferences. The ThemeProvider is configured with attribute="class" to ensure proper class switching.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/dark-mode.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { Theme } from "@radix-ui/themes";
import { ThemeProvider } from "next-themes";

export default function () {
	return (
		<ThemeProvider attribute="class">
			<Theme>
				<MyApp />
			</Theme>
		</ThemeProvider>
	);
}
```

----------------------------------------

TITLE: Rendering Text as Different HTML Elements in JSX
DESCRIPTION: Example showing how to use the 'as' prop to render the Text component as different HTML elements like paragraph, label, div, or span, while maintaining the same styling.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/text.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Text as="p">This is a <Strong>paragraph</Strong> element.</Text>
<Text as="label">This is a <Strong>label</Strong> element.</Text>
<Text as="div">This is a <Strong>div</Strong> element.</Text>
<Text as="span">This is a <Strong>span</Strong> element.</Text>
```

----------------------------------------

TITLE: Toolbar with DropdownMenu Example
DESCRIPTION: This code snippet demonstrates how to compose a Radix UI Toolbar with other primitives, specifically the DropdownMenu component. It showcases how to use the `asChild` prop to integrate the DropdownMenu's Trigger within a Toolbar.Button.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/toolbar.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
import { Toolbar, DropdownMenu } from "radix-ui";

export default () => (
	<Toolbar.Root>
		<Toolbar.Button>Action 1</Toolbar.Button>
		<Toolbar.Separator />
		<DropdownMenu.Root>
			<Toolbar.Button __asChild__>
				<DropdownMenu.Trigger>Trigger</DropdownMenu.Trigger>
			</Toolbar.Button>
			<DropdownMenu.Content>…</DropdownMenu.Content>
		</DropdownMenu.Root>
	</Toolbar.Root>
);
```

----------------------------------------

TITLE: Composing Tooltip with asChild in Radix UI using JSX
DESCRIPTION: This snippet demonstrates how to utilize the asChild prop with Radix UI's Tooltip.Trigger to render a link instead of the default button. It highlights the importance of ensuring the custom element remains accessible and functional. The code imports React and Tooltip components, and overwrites the default DOM element using asChild.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/guides/composition.mdx#2025-04-21_snippet_0

LANGUAGE: JavaScript
CODE:
```
import * as React from "react";
import { Tooltip } from "radix-ui";

export default () => (
	<Tooltip.Root>
		<Tooltip.Trigger asChild>
			<a href="https://www.radix-ui.com/">Radix UI</a>
		</Tooltip.Trigger>
		<Tooltip.Portal>…</Tooltip.Portal>
	</Tooltip.Root>
);
```

----------------------------------------

TITLE: Dropdown Menu Structure - React
DESCRIPTION: This snippet demonstrates how to set up the basic structure of a dropdown menu component in React using Radix UI. It includes the Root, Trigger, and various menu parts like Content, Label, Item, and others.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/dropdown-menu.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { DropdownMenu } from "radix-ui";

export default () => (
	<DropdownMenu.Root>
		<DropdownMenu.Trigger />

		<DropdownMenu.Portal>
			<DropdownMenu.Content>
				<DropdownMenu.Label />
				<DropdownMenu.Item />

				<DropdownMenu.Group>
					<DropdownMenu.Item />
				</DropdownMenu.Group>

				<DropdownMenu.CheckboxItem>
					<DropdownMenu.ItemIndicator />
				</DropdownMenu.CheckboxItem>

				<DropdownMenu.RadioGroup>
					<DropdownMenu.RadioItem>
						<DropdownMenu.ItemIndicator />
					</DropdownMenu.RadioItem>
				</DropdownMenu.RadioGroup>

				<DropdownMenu.Sub>
					<DropdownMenu.SubTrigger />
					<DropdownMenu.Portal>
						<DropdownMenu.SubContent />
					</DropdownMenu.Portal>
				</DropdownMenu.Sub>

				<DropdownMenu.Separator />
				<DropdownMenu.Arrow />
			</DropdownMenu.Content>
			</DropdownMenu.Portal>
	</DropdownMenu.Root>
);
```

----------------------------------------

TITLE: Handling Button Loading States in JSX
DESCRIPTION: This snippet outlines using the 'loading' prop in JSX to show a spinner during button's loading state. The buttons maintain original size while loading, and are disabled to prevent user interaction, demonstrated across different variants.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/button.mdx#2025-04-21_snippet_8

LANGUAGE: jsx
CODE:
```
<Flex gap="3">
	<Button loading variant="classic">
		Bookmark
	</Button>
	<Button loading variant="solid">
		Bookmark
	</Button>
	<Button loading variant="soft">
		Bookmark
	</Button>
	<Button loading variant="surface">
		Bookmark
	</Button>
	<Button loading variant="outline">
		Bookmark
	</Button>
</Flex>
```

----------------------------------------

TITLE: Menubar Component Anatomy in React
DESCRIPTION: Example of how to import and structure the Menubar component with all its subcomponents in a React application.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/menubar.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { Menubar } from "radix-ui";

export default () => (
	<Menubar.Root>
		<Menubar.Menu>
			<Menubar.Trigger />
			<Menubar.Portal>
				<Menubar.Content>
					<Menubar.Label />
					<Menubar.Item />

					<Menubar.Group>
						<Menubar.Item />
					</Menubar.Group>

					<Menubar.CheckboxItem>
						<Menubar.ItemIndicator />
					</Menubar.CheckboxItem>

					<Menubar.RadioGroup>
						<Menubar.RadioItem>
							<Menubar.ItemIndicator />
						</Menubar.RadioItem>
					</Menubar.RadioGroup>

					<Menubar.Sub>
						<Menubar.SubTrigger />
						<Menubar.Portal>
							<Menubar.SubContent />
						</Menubar.Portal>
					</Menubar.Sub>

					<Menubar.Separator />
					<Menubar.Arrow />
				</Menubar.Content>
			</Menubar.Portal>
		</Menubar.Menu>
	</Menubar.Root>
);
```

----------------------------------------

TITLE: Implementing Basic Hover Card Structure in React
DESCRIPTION: Example showing the anatomy of a Hover Card component with all its constituent parts properly structured in JSX.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/hover-card.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { HoverCard } from "radix-ui";

export default () => (
	<HoverCard.Root>
		<HoverCard.Trigger />
		<HoverCard.Portal>
			<HoverCard.Content>
				<HoverCard.Arrow />
			</HoverCard.Content>
		</HoverCard.Portal>
	</HoverCard.Root>
);
```

----------------------------------------

TITLE: Rendering Progress Component with JSX
DESCRIPTION: The JSX code snippet demonstrates how to import the Progress component from Radix UI and render it within a React component. It highlights setting up the Progress.Root and Progress.Indicator components to visualize task progress.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/progress.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { Progress } from "radix-ui";

export default () => (
	<Progress.Root>
		<Progress.Indicator />
	</Progress.Root>
);
```

----------------------------------------

TITLE: Customizing Radix Themes with Theme Component
DESCRIPTION: Example of how to customize the appearance of Radix Themes by passing configuration to the Theme component.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/overview/getting-started.mdx#2025-04-21_snippet_6

LANGUAGE: jsx
CODE:
```
<Theme accentColor="crimson" grayColor="sand" radius="large" scaling="95%">
	<MyApp />
</Theme>
```

----------------------------------------

TITLE: Basic Icon Button in JSX
DESCRIPTION: This snippet demonstrates the basic usage of the `IconButton` component with a `MagnifyingGlassIcon` inside. It shows how to create a simple icon button using the Radix UI library.  The MagnifyingGlassIcon is likely a component defined elsewhere and imported for use here.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/icon-button.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
"<IconButton>\n\t<MagnifyingGlassIcon width=\"18\" height=\"18\" />\n</IconButton>"
```

----------------------------------------

TITLE: Creating Basic Dropdown Menu Structure with Radix UI
DESCRIPTION: This snippet demonstrates the basic structure of a dropdown menu using Radix UI's DropdownMenu components, including items and submenus. It utilizes the DropdownMenu.Root, Trigger, Content, Item, Sub, SubTrigger, and SubContent components to create a hierarchical dropdown menu.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/dropdown-menu.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<DropdownMenu.Root>
	<DropdownMenu.Trigger>
		<Button variant="soft">
			Options
			<DropdownMenu.TriggerIcon />
		</Button>
	</DropdownMenu.Trigger>
	<DropdownMenu.Content>
		<DropdownMenu.Item shortcut="⌘ E">Edit</DropdownMenu.Item>
		<DropdownMenu.Item shortcut="⌘ D">Duplicate</DropdownMenu.Item>
		<DropdownMenu.Separator />
		<DropdownMenu.Item shortcut="⌘ N">Archive</DropdownMenu.Item>

		<DropdownMenu.Sub>
			<DropdownMenu.SubTrigger>More</DropdownMenu.SubTrigger>
			<DropdownMenu.SubContent>
				<DropdownMenu.Item>Move to project…</DropdownMenu.Item>
				<DropdownMenu.Item>Move to folder…</DropdownMenu.Item>

				<DropdownMenu.Separator />
				<DropdownMenu.Item>Advanced options…</DropdownMenu.Item>
			</DropdownMenu.SubContent>
		</DropdownMenu.Sub>

		<DropdownMenu.Separator />
		<DropdownMenu.Item>Share</DropdownMenu.Item>
		<DropdownMenu.Item>Add to favorites</DropdownMenu.Item>
		<DropdownMenu.Separator />
		<DropdownMenu.Item shortcut="⌘ ⌫" color="red">
			Delete
		</DropdownMenu.Item>
	</DropdownMenu.Content>
</DropdownMenu.Root>
```

----------------------------------------

TITLE: Implementing Popover Component Structure in React
DESCRIPTION: Example of how to import and structure all parts of the Popover component in a React application. Shows the basic composition of Root, Trigger, Anchor, Portal, Content, Close, and Arrow components.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/popover.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { Popover } from "radix-ui";

export default () => (
	<Popover.Root>
		<Popover.Trigger />
		<Popover.Anchor />
		<Popover.Portal>
			<Popover.Content>
				<Popover.Close />
				<Popover.Arrow />
			</Popover.Content>
		</Popover.Portal>
	</Popover.Root>
);
```

----------------------------------------

TITLE: Basic Switch Component in JSX
DESCRIPTION: Renders a basic switch component with the defaultChecked prop set to true.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/switch.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Switch defaultChecked />
```

----------------------------------------

TITLE: Card as a Link with 'asChild' Prop - JSX
DESCRIPTION: This snippet demonstrates how to render the Radix UI Card component as a link using the `asChild` prop.  This is useful for creating interactive cards that navigate to other pages or sections.  Interactive states like hover and focus are automatically styled.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/card.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
"<Box maxWidth=\"350px\">\n\t<Card asChild>\n\t	<a href=\"#\">\n\t		<Text as=\"div\" size=\"2\" weight=\"bold\">\n\t			Quick start\n\t		</Text>\n\t		<Text as=\"div\" color=\"gray\" size=\"2\">\n\t			Start building your next project in minutes\n\t		</Text>\n\t	</a>\n\t</Card>\n</Box>"
```

----------------------------------------

TITLE: Basic TextArea Implementation
DESCRIPTION: Simple implementation of a TextArea component with a placeholder.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/text-area.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<TextArea placeholder="Reply to comment…" />
```

----------------------------------------

TITLE: Initializing Basic Context Menu in React
DESCRIPTION: Demonstrates the basic structure of a Context Menu with multiple items, separators, and a submenu. Includes keyboard shortcuts and supports nested menu interactions.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/context-menu.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<ContextMenu.Root>\n\t<ContextMenu.Trigger>\n\t\t<RightClickZone style={{ height: 150 }} />\n\t</ContextMenu.Trigger>\n\t<ContextMenu.Content>\n\t\t<ContextMenu.Item shortcut="⌘ E">Edit</ContextMenu.Item>\n\t\t<ContextMenu.Item shortcut="⌘ D">Duplicate</ContextMenu.Item>\n\t\t<ContextMenu.Separator />\n\t\t<ContextMenu.Item shortcut="⌘ N">Archive</ContextMenu.Item>\n\n\t\t<ContextMenu.Sub>\n\t\t\t<ContextMenu.SubTrigger>More</ContextMenu.SubTrigger>\n\t\t\t<ContextMenu.SubContent>\n\t\t\t\t<ContextMenu.Item>Move to project…</ContextMenu.Item>\n\t\t\t\t<ContextMenu.Item>Move to folder…</ContextMenu.Item>\n\t\t\t\t<ContextMenu.Separator />\n\t\t\t\t<ContextMenu.Item>Advanced options…</ContextMenu.Item>\n\t\t\t</ContextMenu.SubContent>\n\t\t</ContextMenu.Sub>\n\n\t\t<ContextMenu.Separator />\n\t\t<ContextMenu.Item>Share</ContextMenu.Item>\n\t\t<ContextMenu.Item>Add to favorites</ContextMenu.Item>\n\t\t<ContextMenu.Separator />\n\t\t<ContextMenu.Item shortcut="⌘ ⌫" color="red">\n\t\t\tDelete\n\t\t</ContextMenu.Item>\n\t</ContextMenu.Content>\n</ContextMenu.Root>
```

----------------------------------------

TITLE: Applying dark mode class in HTML
DESCRIPTION: This snippet shows how to apply the `.dark` class to the `<body>` element (or a parent element) to enable dark mode styling using Radix Colors.
SOURCE: https://github.com/radix-ui/website/blob/main/data/colors/docs/overview/usage.mdx#2025-04-21_snippet_1

LANGUAGE: html
CODE:
```
<!-- For dark mode, apply a `.dark` class to <body> or a parent. -->
<body class="dark">
	<button class="button">Button</button>
</body>
```

----------------------------------------

TITLE: Implementing SSR-compatible Select in React
DESCRIPTION: This example shows how to implement a server-side rendering compatible Select component by manually rendering the selected item's text to avoid layout shifts after hydration.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/select.mdx#2025-04-21_snippet_9

LANGUAGE: jsx
CODE:
```
() => {
	const data = {
		apple: "Apple",
		orange: "Orange",
	};
	const [value, setValue] = React.useState("apple");
	return (
		<Select.Root value={value} onValueChange={setValue}>
			<Select.Trigger>{data[value]}</Select.Trigger>
			<Select.Content>
				<Select.Item value="apple">Apple</Select.Item>
				<Select.Item value="orange">Orange</Select.Item>
			</Select.Content>
		</Select.Root>
	);
};
```

----------------------------------------

TITLE: Basic Tabs Implementation in React
DESCRIPTION: Demonstrates the basic structure of a tabbed interface with three sections: Account, Documents, and Settings. Shows how to set up tab triggers and their associated content.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/tabs.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Tabs.Root defaultValue="account">
	<Tabs.List>
		<Tabs.Trigger value="account">Account</Tabs.Trigger>
		<Tabs.Trigger value="documents">Documents</Tabs.Trigger>
		<Tabs.Trigger value="settings">Settings</Tabs.Trigger>
	</Tabs.List>

	<Box pt="3">
		<Tabs.Content value="account">
			<Text size="2">Make changes to your account.</Text>
		</Tabs.Content>

		<Tabs.Content value="documents">
			<Text size="2">Access and update your documents.</Text>
		</Tabs.Content>

		<Tabs.Content value="settings">
			<Text size="2">Edit your profile or update contact information.</Text>
		</Tabs.Content>
	</Box>
</Tabs.Root>
```

----------------------------------------

TITLE: Basic Table Structure in React with Radix UI
DESCRIPTION: A basic example of a Radix UI Table component showing a structured data table with headers and rows containing user information.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/table.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Table.Root>
	<Table.Header>
		<Table.Row>
			<Table.ColumnHeaderCell>Full name</Table.ColumnHeaderCell>
			<Table.ColumnHeaderCell>Email</Table.ColumnHeaderCell>
			<Table.ColumnHeaderCell>Group</Table.ColumnHeaderCell>
		</Table.Row>
	</Table.Header>

	<Table.Body>
		<Table.Row>
			<Table.RowHeaderCell>Danilo Sousa</Table.RowHeaderCell>
			<Table.Cell>danilo@example.com</Table.Cell>
			<Table.Cell>Developer</Table.Cell>
		</Table.Row>

		<Table.Row>
			<Table.RowHeaderCell>Zahra Ambessa</Table.RowHeaderCell>
			<Table.Cell>zahra@example.com</Table.Cell>
			<Table.Cell>Admin</Table.Cell>
		</Table.Row>

		<Table.Row>
			<Table.RowHeaderCell>Jasper Eriksson</Table.RowHeaderCell>
			<Table.Cell>jasper@example.com</Table.Cell>
			<Table.Cell>Developer</Table.Cell>
		</Table.Row>
	</Table.Body>
</Table.Root>
```

----------------------------------------

TITLE: Implementing Responsive Heading with Breakpoints in React/JSX
DESCRIPTION: Example showing how to use the Responsive object shape to modify component props across different breakpoints. The Heading component's size changes based on screen width breakpoints: initial (default), md (medium), and xl (extra large).
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/breakpoints.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Heading
	size={{
		initial: "3",
		md: "5",
		xl: "7",
	}}
/>
```

----------------------------------------

TITLE: Anatomy of a Dialog Component
DESCRIPTION: Shows the basic structure of a Dialog component by importing and assembling all the necessary parts including Root, Trigger, Portal, Overlay, Content, Title, Description, and Close.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/dialog.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { Dialog } from "radix-ui";

export default () => (
	<Dialog.Root>
		<Dialog.Trigger />
		<Dialog.Portal>
			<Dialog.Overlay />
			<Dialog.Content>
				<Dialog.Title />
				<Dialog.Description />
				<Dialog.Close />
			</Dialog.Content>
		</Dialog.Portal>
	</Dialog.Root>
);
```

----------------------------------------

TITLE: Configuring Theme Component - Radix UI JSX
DESCRIPTION: Demonstrates basic configuration of the Theme component with customization options for accent color, gray color, panel background, scaling, and radius.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/overview.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Theme
	accentColor="mint"
	grayColor="gray"
	panelBackground="solid"
	scaling="100%"
	radius="full"
>
	<ThemesVolumeControlExample />
</Theme>
```

----------------------------------------

TITLE: Importing and using Radix Colors in Vanilla CSS
DESCRIPTION: This snippet demonstrates how to import Radix Colors CSS files and use the colors as CSS variables. It includes examples for both light and dark themes. The light scales apply to `:root` and `.light` classes, while dark scales apply to `.dark` class.
SOURCE: https://github.com/radix-ui/website/blob/main/data/colors/docs/overview/usage.mdx#2025-04-21_snippet_0

LANGUAGE: css
CODE:
```
/* Import only the scales you need */
@import "@radix-ui/colors/gray.css";
@import "@radix-ui/colors/blue.css";
@import "@radix-ui/colors/green.css";
@import "@radix-ui/colors/red.css";
@import "@radix-ui/colors/gray-dark.css";
@import "@radix-ui/colors/blue-dark.css";
@import "@radix-ui/colors/green-dark.css";
@import "@radix-ui/colors/red-dark.css";

/* Use the colors as CSS variables */
.button {
	background-color: var(--blue-4);
	color: var(--blue-11);
	border-color: var(--blue-7);
}
.button:hover {
	background-color: var(--blue-5);
	border-color: var(--blue-8);
}
```

----------------------------------------

TITLE: Creating a Profile Edit Dialog with Radix UI
DESCRIPTION: This snippet demonstrates how to create a modal dialog for editing user profiles using Radix UI components. It utilizes Dialog components for structuring the modal and includes fields for user input, such as name and email, along with action buttons for canceling and saving changes.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/dialog.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Dialog.Root>
	<Dialog.Trigger>
		<Button>Edit profile</Button>
	</Dialog.Trigger>

	<Dialog.Content maxWidth="450px">
		<Dialog.Title>Edit profile</Dialog.Title>
		<Dialog.Description size="2" mb="4">
			Make changes to your profile.
		</Dialog.Description>

		<Flex direction="column" gap="3">
			<label>
				<Text as="div" size="2" mb="1" weight="bold">
					Name
				</Text>
				<TextField.Root
					defaultValue="Freja Johnsen"
					placeholder="Enter your full name"
				/>
			</label>
			<label>
				<Text as="div" size="2" mb="1" weight="bold">
					Email
				</Text>
				<TextField.Root
					defaultValue="freja@example.com"
					placeholder="Enter your email"
				/>
			</label>
		</Flex>

		<Flex gap="3" mt="4" justify="end">
			<Dialog.Close>
				<Button variant="soft" color="gray">
					Cancel
				</Button>
			</Dialog.Close>
			<Dialog.Close>
				<Button>Save</Button>
			</Dialog.Close>
		</Flex>
	</Dialog.Content>
</Dialog.Root>
```

----------------------------------------

TITLE: Basic Usage of PasswordToggleField with Icon (JSX)
DESCRIPTION: Demonstrates the basic structure of the PasswordToggleField component, including the Input, Toggle, and Icon sub-components. The Icon component uses the 'visible' and 'hidden' props to display different SVG icons based on the field's visibility state.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/password-toggle-field.mdx#_snippet_1

LANGUAGE: jsx
CODE:
```
<PasswordToggleField.Root>
	<PasswordToggleField.Input />
	<PasswordToggleField.Toggle>
		<PasswordToggleField.Icon
			visible={<EyeOpenIcon />}
			hidden={<EyeClosedIcon />}
		/>
	</PasswordToggleField.Toggle>
</PasswordToggleField.Root>
```

----------------------------------------

TITLE: Initializing Radio Group Component in JSX
DESCRIPTION: Demonstrates the basic usage of the Radio Group component with default values and multiple items.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/radio-group.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<RadioGroup.Root defaultValue="1" name="example">
	<RadioGroup.Item value="1">Default</RadioGroup.Item>
	<RadioGroup.Item value="2">Comfortable</RadioGroup.Item>
	<RadioGroup.Item value="3">Compact</RadioGroup.Item>
</RadioGroup.Root>
```

----------------------------------------

TITLE: Controlled Toggle Group Example
DESCRIPTION: This example demonstrates how to create a controlled ToggleGroup component in React. The `value` and `onValueChange` props are used to manage the state of the ToggleGroup, ensuring there's always a selected value.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/toggle-group.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
import * as React from "react";
import { ToggleGroup } from "radix-ui";

export default () => {
	const [value, setValue] = React.useState("left");

	return (
		<ToggleGroup.Root
			type="single"
			value={value}
			onValueChange={(value) => {
				if (value) setValue(value);
			}}
		>
			<ToggleGroup.Item value="left">
				<TextAlignLeftIcon />
			</ToggleGroup.Item>
			<ToggleGroup.Item value="center">
				<TextAlignCenterIcon />
			</ToggleGroup.Item>
			<ToggleGroup.Item value="right">
				<TextAlignRightIcon />
			</ToggleGroup.Item>
		</ToggleGroup.Root>
	);
};

```

----------------------------------------

TITLE: Creating Custom Dialog API - Usage - JSX
DESCRIPTION: This snippet illustrates how to create a custom dialog API by abstracting the primitive components provided by Radix UI. It simplifies the usage of the dialog by encapsulating functionality within custom components. The inputs are custom dialog components, while the output is a simplified dialog structure for users.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/dialog.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
import { Dialog, DialogTrigger, DialogContent } from "./your-dialog";

export default () => (
	<Dialog>
		<DialogTrigger>Dialog trigger</DialogTrigger>
		<DialogContent>Dialog Content</DialogContent>
	</Dialog>
);
```

----------------------------------------

TITLE: Applying Color to Buttons in JSX
DESCRIPTION: This snippet shows how to use the 'color' prop to change the color of buttons in JSX. It showcases buttons in four different colors (indigo, cyan, orange, crimson), using a 'soft' variant. Dependencies include JSX with Flex layout support.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/button.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
<Flex gap="3">
	<Button color="indigo" variant="soft">
		Edit profile
	</Button>
	<Button color="cyan" variant="soft">
		Edit profile
	</Button>
	<Button color="orange" variant="soft">
		Edit profile
	</Button>
	<Button color="crimson" variant="soft">
		Edit profile
	</Button>
</Flex>
```

----------------------------------------

TITLE: Enable High Contrast on DataList Labels with Radix UI in JSX
DESCRIPTION: Shows how to enhance the visibility of labels by using the 'highContrast' prop in Radix UI DataList. This modification improves label visibility against various backgrounds. Perfect for accessibility-minded designs. It uses Radix UI components with the essential highContrast prop.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/data-list.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
<Flex gap="9">
	<DataList.Root orientation="vertical">
		<DataList.Item>
			<DataList.Label color="indigo">Name</DataList.Label>
			<DataList.Value>Indigo</DataList.Value>
		</DataList.Item>
		<DataList.Item>
			<DataList.Label color="cyan">Name</DataList.Label>
			<DataList.Value>Cyan</DataList.Value>
		</DataList.Item>
		<DataList.Item>
			<DataList.Label color="orange">Name</DataList.Label>
			<DataList.Value>Orange</DataList.Value>
		</DataList.Item>
		<DataList.Item>
			<DataList.Label color="crimson">Name</DataList.Label>
			<DataList.Value>Crimson</DataList.Value>
		</DataList.Item>
	</DataList.Root>

	<DataList.Root orientation="vertical">
		<DataList.Item>
			<DataList.Label color="indigo" highContrast>
				Name
			</DataList.Label>
			<DataList.Value>Indigo</DataList.Value>
		</DataList.Item>
		<DataList.Item>
			<DataList.Label color="cyan" highContrast>
				Name
			</DataList.Label>
			<DataList.Value>Cyan</DataList.Value>
		</DataList.Item>
		<DataList.Item>
			<DataList.Label color="orange" highContrast>
				Name
			</DataList.Label>
			<DataList.Value>Orange</DataList.Value>
		</DataList.Item>
		<DataList.Item>
			<DataList.Label color="crimson" highContrast>
				Name
			</DataList.Label>
			<DataList.Value>Crimson</DataList.Value>
		</DataList.Item>
	</DataList.Root>
</Flex>
```

----------------------------------------

TITLE: Composing Form with Custom Components
DESCRIPTION: Demonstrates how to use the asChild prop to compose Form parts with custom components.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/form.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Form.Field name="name">
	<Form.Label>Full name</Form.Label>
	<Form.Control __asChild__>
		<TextField.Input variant="primary" />
	</Form.Control>
</Form.Field>
```

----------------------------------------

TITLE: Rendering Basic Radio Buttons in JSX
DESCRIPTION: This snippet demonstrates how to render a basic set of radio buttons using the Radio component from Radix UI. It shows three radio buttons with different labels arranged vertically.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/radio.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Flex align="start" direction="column" gap="1">
	<Flex asChild gap="2">
		<Text as="label" size="2">
			<Radio name="example" value="1" defaultChecked />
			Default
		</Text>
	</Flex>

	<Flex asChild gap="2">
		<Text as="label" size="2">
			<Radio name="example" value="2" />
			Comfortable
		</Text>
	</Flex>

	<Flex asChild gap="2">
		<Text as="label" size="2">
			<Radio name="example" value="3" />
			Compact
		</Text>
	</Flex>
</Flex>
```

----------------------------------------

TITLE: Styling a Radix UI Accordion Item with CSS
DESCRIPTION: This code snippet demonstrates how to style a Radix UI Accordion Item component using CSS classes. It imports the Accordion component and assigns a className to the Accordion.Item.  A CSS file then targets this className to apply the desired styling.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/guides/styling.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
import * as React from "react";
import { Accordion } from "radix-ui";
import "./styles.css";

const AccordionDemo = () => (
	<Accordion.Root>
		<Accordion.Item __className__="AccordionItem" value="item-1" />
		{/* … */}
	</Accordion.Root>
);

export default AccordionDemo;
```

----------------------------------------

TITLE: Creating a Responsive Grid Layout with Radix UI
DESCRIPTION: This snippet showcases the responsive capabilities of the Radix UI Grid component. It uses a breakpoint object to define the number of columns, setting it to 1 initially and transitioning to 2 columns at the medium breakpoint (md). The gap between items is set to 3 units, and the width is set to auto.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/grid.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
"<Grid columns={{ initial: \"1\", md: \"2\" }} gap=\"3\" width=\"auto\">\n\t<Box height=\"64px\">\n\t\t<DecorativeBox />\n\t</Box>\n\t<Box height=\"64px\">\n\t\t<DecorativeBox />\n\t</Box>\n</Grid>"
```

----------------------------------------

TITLE: Forwarding Refs in Custom React Component using React.forwardRef
DESCRIPTION: This snippet illustrates the process of forwarding refs in a React component using React.forwardRef to ensure compatibility with Radix UI. The code modifies a custom button component to accept and pass a ref for scenarios where Radix might need to access the element's DOM node.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/guides/composition.mdx#2025-04-21_snippet_2

LANGUAGE: JavaScript
CODE:
```
// before
const MyButton = (props) => <button {...props} />;

// after
const MyButton = React.forwardRef((props, forwardedRef) => (
	<button {...props} ref={forwardedRef} />
));
```

----------------------------------------

TITLE: Using Strong Component for Text Emphasis in JSX
DESCRIPTION: Basic example showing how to use the Strong component to emphasize text within a Text component.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/strong.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Text>
	The most important thing to remember is, <Strong>stay positive</Strong>.
</Text>
```

----------------------------------------

TITLE: Styling Disabled DropdownMenu Items in React and CSS
DESCRIPTION: This example shows how to add special styles to disabled items in a DropdownMenu using the data-disabled attribute.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/dropdown-menu.mdx#2025-04-21_snippet_8

LANGUAGE: jsx
CODE:
```
// index.jsx
import { DropdownMenu } from "radix-ui";
import "./styles.css";

export default () => (
	<DropdownMenu.Root>
		<DropdownMenu.Trigger>…</DropdownMenu.Trigger>
		<DropdownMenu.Portal>
			<DropdownMenu.Content>
				<DropdownMenu.Item __className__="DropdownMenuItem" __disabled__>
					…
				</DropdownMenu.Item>
				<DropdownMenu.Item className="DropdownMenuItem">…</DropdownMenu.Item>
			</DropdownMenu.Content>
		</DropdownMenu.Portal>
	</DropdownMenu.Root>
);
```

LANGUAGE: css
CODE:
```
/* styles.css */
.DropdownMenuItem[__data-disabled__] {
	color: gainsboro;
}
```

----------------------------------------

TITLE: Server-Side Form Validation with Radix UI
DESCRIPTION: Comprehensive example of handling server-side validation, including error state management, dynamic error messages, and clearing server errors
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/form.mdx#2025-04-21_snippet_8

LANGUAGE: jsx
CODE:
```
import * as React from "react";
import { Form } from "radix-ui";

function Page() {
	const [serverErrors, setServerErrors] = React.useState({
		email: false,
		password: false,
	});

	return (
		<Form.Root
			onSubmit={(event) => {
				const data = Object.fromEntries(new FormData(event.currentTarget));

				submitForm(data)
					.then(() => {})
					.catch((errors) => __setServerErrors__(mapServerErrors(errors)));

				event.preventDefault();
			}}
			onClearServerErrors={() =>
				__setServerErrors__({ email: false, password: false })
			}
		>
			<Form.Field name="email" __serverInvalid__={serverErrors.email}>
				<Form.Label>Email address</Form.Label>
				<Form.Control type="email" required />
				<Form.Message match="valueMissing">
					Please enter your email.
				</Form.Message>
				<Form.Message match="typeMismatch" __forceMatch__={serverErrors.email}>
					Please provide a valid email.
				</Form.Message>
			</Form.Field>

			<Form.Field name="password" __serverInvalid__={serverErrors.password}>
				<Form.Label>Password</Form.Label>
				<Form.Control type="password" required />
				<Form.Message match="valueMissing">
					Please enter a password.
				</Form.Message>
				{serverErrors.password && (
					<Form.Message>
						Please provide a valid password. It should contain at least 1 number
						and 1 special character.
					</Form.Message>
				)}
			</Form.Field>

			<Form.Submit>Submit</Form.Submit>
		</Form.Root>
	);
}
```

LANGUAGE: jsx
CODE:
```
<Form.Field name="email" serverInvalid={serverErrors.email}>
	<Form.Label>Email address</Form.Label>
	<Form.Control
		type="email"
		__onChange__={() => setServerErrors((prev) => ({ ...prev, email: false }))}
	/>
	<Form.Message match="valueMissing">Please enter your email.</Form.Message>
	<Form.Message match="typeMismatch" forceMatch={serverErrors.email}>
		Please provide a valid email.
	</Form.Message>
</Form.Field>
```

----------------------------------------

TITLE: Configuring Button Sizes in JSX
DESCRIPTION: This code snippet illustrates how to use the 'size' prop to control the button's size in JSX. The Button component varies in size from '1' to '3'. The snippet requires no additional dependencies and outputs Button elements in different sizes based on the provided prop.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/button.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Flex gap="3" align="center">
	<Button size="1" variant="soft">
		Edit profile
	</Button>
	<Button size="2" variant="soft">
		Edit profile
	</Button>
	<Button size="3" variant="soft">
		Edit profile
	</Button>
</Flex>
```

----------------------------------------

TITLE: Conditional Skeleton Loading State
DESCRIPTION: Demonstrates using the loading prop to toggle between skeleton and actual content, preserving dimensions and managing interactive elements.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/skeleton.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Flex gap="4">
	<Skeleton loading={true}>
		<Switch defaultChecked />
	</Skeleton>

	<Skeleton loading={false}>
		<Switch defaultChecked />
	</Skeleton>
</Flex>
```

----------------------------------------

TITLE: Creating a Button with Icon in JSX
DESCRIPTION: This snippet demonstrates how to create a button with an icon using the Button component in JSX. It uses a nested BookmarkIcon component within the Button component. No dependencies are required beyond JSX support, and typical usage involves wrapping text or other inline elements. The input is the JSX markup, and the output is a Button element rendered with an icon.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/button.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Button>
	<BookmarkIcon /> Bookmark
</Button>
```

----------------------------------------

TITLE: Implementing Checkbox Items in DropdownMenu with React
DESCRIPTION: This snippet demonstrates how to add checkbox items to a DropdownMenu using the CheckboxItem part and managing its state with React hooks.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/dropdown-menu.mdx#2025-04-21_snippet_11

LANGUAGE: jsx
CODE:
```
import * as React from "react";
import { CheckIcon } from "@radix-ui/react-icons";
import { DropdownMenu } from "radix-ui";

export default () => {
	const [checked, setChecked] = React.useState(true);

	return (
		<DropdownMenu.Root>
			<DropdownMenu.Trigger>…</DropdownMenu.Trigger>
			<DropdownMenu.Portal>
				<DropdownMenu.Content>
					<DropdownMenu.Item>…</DropdownMenu.Item>
					<DropdownMenu.Item>…</DropdownMenu.Item>
					<DropdownMenu.Separator />
					<DropdownMenu.CheckboxItem
						checked={checked}
						onCheckedChange={setChecked}
					>
						<DropdownMenu.ItemIndicator>
							<CheckIcon />
						</DropdownMenu.ItemIndicator>
						Checkbox item
					</DropdownMenu.CheckboxItem>
				</DropdownMenu.Content>
			</DropdownMenu.Portal>
		</DropdownMenu.Root>
	);
};
```

----------------------------------------

TITLE: Creating Semantic Color Aliases in JavaScript with Radix Colors
DESCRIPTION: This snippet shows how to import Radix color scales in a JavaScript environment and create a theme object with semantic aliases. It demonstrates mapping color scale values to purpose-based property names for accent, success, warning, and danger colors.
SOURCE: https://github.com/radix-ui/website/blob/main/data/colors/docs/overview/aliasing.mdx#2025-04-21_snippet_1

LANGUAGE: javascript
CODE:
```
import {
	blue,
	green,
	yellow,
	red
} from "@radix-ui/colors";

const theme = {
	...blue,
	...green,
	...yellow,
	...red,

	accent1: blue.blue1,
	accent2: blue.blue2,
	accent3: blue.blue3,
	accent4: blue.blue4,
	accent5: blue.blue5,
	accent6: blue.blue6,
	accent7: blue.blue7,
	accent8: blue.blue8,
	accent9: blue.blue9,
	accent10: blue.blue10,
	accent11: blue.blue11,
	accent12: blue.blue12,

	success1: green.green1,
	success2: green.green2,
	// repeat for all steps

	warning1: yellow.yellow1,
	warning2: yellow.yellow2,
	// repeat for all steps

	danger1: red.red1,
	danger2: red.red2,
	// repeat for all steps
};
```

----------------------------------------

TITLE: Implementing Solid Variant Buttons with Full Color Spectrum in JSX
DESCRIPTION: This code displays solid variant buttons across the full Radix color spectrum. It demonstrates the application of steps 9-10 for solid backgrounds, where step 9 is used for normal states and step 10 for hover states.
SOURCE: https://github.com/radix-ui/website/blob/main/data/colors/docs/palette-composition/understanding-the-scale.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Grid columns="7" gap="3" my="5">
	<Button variant="solid" color="gold">
		Gold
	</Button>
	<Button variant="solid" color="bronze">
		Bronze
	</Button>
	<Button variant="solid" color="brown">
		Brown
	</Button>
	<Button variant="solid" color="yellow">
		Yellow
	</Button>
	<Button variant="solid" color="amber">
		Amber
	</Button>
	<Button variant="solid" color="orange">
		Orange
	</Button>
	<Button variant="solid" color="tomato">
		Tomato
	</Button>
	<Button variant="solid" color="red">
		Red
	</Button>
	<Button variant="solid" color="ruby">
		Ruby
	</Button>
	<Button variant="solid" color="crimson">
		Crimson
	</Button>
	<Button variant="solid" color="pink">
		Pink
	</Button>
	<Button variant="solid" color="plum">
		Plum
	</Button>
	<Button variant="solid" color="purple">
		Purple
	</Button>
	<Button variant="solid" color="violet">
		Violet
	</Button>
	<Button variant="solid" color="iris">
		Iris
	</Button>
	<Button variant="solid" color="indigo">
		Indigo
	</Button>
	<Button variant="solid" color="blue">
		Blue
	</Button>
	<Button variant="solid" color="cyan">
		Cyan
	</Button>
	<Button variant="solid" color="teal">
		Teal
	</Button>
	<Button variant="solid" color="jade">
		Jade
	</Button>
	<Button variant="solid" color="green">
		Green
	</Button>
	<Button variant="solid" color="grass">
		Grass
	</Button>
	<Button variant="solid" color="lime">
		Lime
	</Button>
	<Button variant="solid" color="mint">
		Mint
	</Button>
	<Button variant="solid" color="sky">
		Sky
	</Button>
</Grid>
```

----------------------------------------

TITLE: Creating a Custom DropdownMenu API with Abstract Components
DESCRIPTION: Shows how to create a custom API by abstracting Radix UI DropdownMenu primitive parts into reusable components, including automatic arrow placement and item indicators for checkbox and radio items.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/dropdown-menu.mdx#2025-04-21_snippet_17

LANGUAGE: jsx
CODE:
```
import {
	DropdownMenu,
	DropdownMenuTrigger,
	DropdownMenuContent,
	DropdownMenuLabel,
	DropdownMenuItem,
	DropdownMenuGroup,
	DropdownMenuCheckboxItem,
	DropdownMenuRadioGroup,
	DropdownMenuRadioItem,
	DropdownMenuSeparator,
} from "./your-dropdown-menu";

export default () => (
	<DropdownMenu>
		<DropdownMenuTrigger>DropdownMenu trigger</DropdownMenuTrigger>
		<DropdownMenuContent>
			<DropdownMenuItem>Item</DropdownMenuItem>
			<DropdownMenuLabel>Label</DropdownMenuLabel>
			<DropdownMenuGroup>Group</DropdownMenuGroup>
			<DropdownMenuCheckboxItem>CheckboxItem</DropdownMenuCheckboxItem>
			<DropdownMenuSeparator>Separator</DropdownMenuSeparator>
			<DropdownMenuRadioGroup>
				<DropdownMenuRadioItem>RadioItem</DropdownMenuRadioItem>
				<DropdownMenuRadioItem>RadioItem</DropdownMenuRadioItem>
			</DropdownMenuRadioGroup>
		</DropdownMenuContent>
	</DropdownMenu>
);
```

LANGUAGE: jsx
CODE:
```
// your-dropdown-menu.jsx
import * as React from "react";
import { DropdownMenu as DropdownMenuPrimitive } from "radix-ui";
import { CheckIcon, DividerHorizontalIcon } from "@radix-ui/react-icons";

export const DropdownMenu = DropdownMenuPrimitive.Root;
export const DropdownMenuTrigger = DropdownMenuPrimitive.Trigger;

export const DropdownMenuContent = React.forwardRef(
	({ children, ...props }, forwardedRef) => {
		return (
			<DropdownMenuPrimitive.Portal>
				<DropdownMenuPrimitive.Content {...props} ref={forwardedRef}>
					{children}
					<DropdownMenuPrimitive.Arrow />
				</DropdownMenuPrimitive.Content>
			</DropdownMenuPrimitive.Portal>
		);
	},
);

export const DropdownMenuLabel = DropdownMenuPrimitive.Label;
export const DropdownMenuItem = DropdownMenuPrimitive.Item;
export const DropdownMenuGroup = DropdownMenuPrimitive.Group;

export const DropdownMenuCheckboxItem = React.forwardRef(
	({ children, ...props }, forwardedRef) => {
		return (
			<DropdownMenuPrimitive.CheckboxItem {...props} ref={forwardedRef}>
				{children}
				<DropdownMenuPrimitive.ItemIndicator>
					{props.checked === "indeterminate" && <DividerHorizontalIcon />}
					{props.checked === true && <CheckIcon />}
				</DropdownMenuPrimitive.ItemIndicator>
			</DropdownMenuPrimitive.CheckboxItem>
		);
	},
);

export const DropdownMenuRadioGroup = DropdownMenuPrimitive.RadioGroup;

export const DropdownMenuRadioItem = React.forwardRef(
	({ children, ...props }, forwardedRef) => {
		return (
			<DropdownMenuPrimitive.RadioItem {...props} ref={forwardedRef}>
				{children}
				<DropdownMenuPrimitive.ItemIndicator>
					<CheckIcon />
				</DropdownMenuPrimitive.ItemIndicator>
			</DropdownMenuPrimitive.RadioItem>
		);
	},
);

export const DropdownMenuSeparator = DropdownMenuPrimitive.Separator;
```

----------------------------------------

TITLE: Checkbox Size Variants in JSX
DESCRIPTION: Demonstrates the use of the size prop to control the size of the Checkbox components rendered in JSX. The example shows three different size settings (size="1", size="2", and size="3") with defaultChecked enabled. Ensure the correct import of Flex and Checkbox components.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/checkbox.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Flex align="center" gap="2">
	<Checkbox size="1" defaultChecked />
	<Checkbox size="2" defaultChecked />
	<Checkbox size="3" defaultChecked />
</Flex>
```

----------------------------------------

TITLE: Creating Tooltip Structure in React
DESCRIPTION: This snippet demonstrates how to create a tooltip structure within a React component using Radix UI. It showcases the proper use of Tooltip.Provider, Tooltip.Root, Tooltip.Trigger, Tooltip.Portal, and Tooltip.Content components to assemble the tooltip functionality.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/tooltip.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { Tooltip } from "radix-ui";

export default () => (
	<Tooltip.Provider>
		<Tooltip.Root>
			<Tooltip.Trigger />
			<Tooltip.Portal>
				<Tooltip.Content>
					<Tooltip.Arrow />
				</Tooltip.Content>
			</Tooltip.Portal>
		</Tooltip.Root>
	</Tooltip.Provider>
);
```

----------------------------------------

TITLE: Implementing radio items in Menubar (Radix UI, React)
DESCRIPTION: This example demonstrates how to create a radio item group in a Radix UI Menubar using `Menubar.RadioGroup` and `Menubar.RadioItem`. The `color` state is managed using `React.useState`, and the `onValueChange` handler updates the state when a radio item is selected. The `Menubar.ItemIndicator` displays a check icon for the selected item.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/menubar.mdx#2025-04-21_snippet_11

LANGUAGE: jsx
CODE:
```
import * as React from "react";
import { CheckIcon } from "@radix-ui/react-icons";
import { Menubar } from "radix-ui";

export default () => {
	const [color, setColor] = React.useState("blue");

	return (
		<Menubar.Root>
			<Menubar.Menu>
				<Menubar.Trigger>…</Menubar.Trigger>
				<Menubar.Portal>
					<Menubar.Content>
						<Menubar.RadioGroup value={color} onValueChange={setColor}>
							<Menubar.RadioItem value="red">
								<Menubar.ItemIndicator>
									<CheckIcon />
								</Menubar.ItemIndicator>
								Red
							</Menubar.RadioItem>
							<Menubar.RadioItem value="blue">
								<Menubar.ItemIndicator>
									<CheckIcon />
								</Menubar.ItemIndicator>
								Blue
							</Menubar.RadioItem>
						</Menubar.RadioGroup>
					</Menubar.Content>
				</Menubar.Portal>
			</Menubar.Menu>
		</Menubar.Root>
	);
};
```

----------------------------------------

TITLE: Basic Link Component
DESCRIPTION: A basic example demonstrating the Radix UI Link component. It showcases a simple link with a default href attribute.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/link.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Link href="#">Sign up</Link>
```

----------------------------------------

TITLE: Extending Radix UI Accordion Primitive with React.forwardRef
DESCRIPTION: This code demonstrates how to extend a Radix UI Accordion primitive using `React.forwardRef`. This allows you to pass a ref to the underlying DOM element of the primitive component.  It also sets the `displayName` for debugging purposes.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/guides/styling.mdx#2025-04-21_snippet_4

LANGUAGE: tsx
CODE:
```
import * as React from "react";
import { Accordion as AccordionPrimitive } from "radix-ui";

const AccordionItem = React.forwardRef<
	React.ElementRef<typeof AccordionPrimitive.Item>,
	React.ComponentPropsWithoutRef<typeof AccordionPrimitive.Item>
>((props, forwardedRef) => (
	<AccordionPrimitive.Item {...props} ref={forwardedRef} />
));
AccordionItem.displayName = "AccordionItem";
```

----------------------------------------

TITLE: Importing Individual Radix UI Primitives - TypeScript
DESCRIPTION: This snippet details how to import specific Radix UI primitives after installing them individually. This allows for selective usage of components in a TypeScript environment.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/overview/introduction.mdx#2025-04-21_snippet_3

LANGUAGE: typescript
CODE:
```
import * as Dialog from "@radix-ui/react-dialog";
import * as DropdownMenu from "@radix-ui/react-dropdown-menu";
import * as Tooltip from "@radix-ui/react-tooltip";
```

----------------------------------------

TITLE: Basic Checkbox Component Structure
DESCRIPTION: Basic implementation showing the anatomy of the Checkbox component with Root and Indicator parts.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/checkbox.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { Checkbox } from "radix-ui";

export default () => (
	<Checkbox.Root>
		<Checkbox.Indicator />
	</Checkbox.Root>
);
```

----------------------------------------

TITLE: Rendering Heading and Text Components in JSX
DESCRIPTION: Demonstrates the use of Heading and Text components to render titles and body copy with consistent typography.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/typography.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Heading mb="2" size="4">Typographic principles</Heading>
<Text>The goal of typography is to relate font size, line height, and line width in a proportional way that maximizes beauty and makes reading easier and more pleasant.</Text>
```

----------------------------------------

TITLE: Adding Theme Component to React Application
DESCRIPTION: Example of how to wrap the root component of a React application with the Theme component from Radix Themes.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/overview/getting-started.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
import { Theme } from "@radix-ui/themes";

export default function () {
	return (
		<html>
			<body>
				<Theme>
					<MyApp />
				</Theme>
			</body>
		</html>
	);
}
```

----------------------------------------

TITLE: Close AlertDialog After Async Operation in React
DESCRIPTION: This example shows how to close an AlertDialog programmatically after completing an asynchronous operation like a form submission. It uses React state management to control the dialog's open state. Dependencies: Radix UI, React.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/alert-dialog.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
import * as React from "react";
import { AlertDialog } from "radix-ui";

const wait = () => new Promise((resolve) => setTimeout(resolve, 1000));

export default () => {
	const [open, setOpen] = React.useState(false);

	return (
		<AlertDialog.Root __open__={open} __onOpenChange__={setOpen}>
			<AlertDialog.Trigger>Open</AlertDialog.Trigger>
			<AlertDialog.Portal>
				<AlertDialog.Overlay />
				<AlertDialog.Content>
					<form
						onSubmit={(event) => {
							wait().then(() => setOpen(false));
							event.preventDefault();
						}}
					>
						{/** some inputs */}
						<button type="submit">Submit</button>
					</form>
				</AlertDialog.Content>
			</AlertDialog.Portal>
		</AlertDialog.Root>
	);
};
```

----------------------------------------

TITLE: Basic Usage of Radix UI PasswordToggleField (JSX)
DESCRIPTION: This snippet shows how to assemble the `PasswordToggleField` component using its constituent parts: `Root`, `Input`, `Toggle`, and `Icon`. It demonstrates importing the component and icons, and structuring the JSX to create a functional password field with a visibility toggle.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/password-toggle-field.mdx#_snippet_0

LANGUAGE: jsx
CODE:
```
import { unstable_PasswordToggleField as PasswordToggleField } from "radix-ui";
import { EyeClosedIcon, EyeOpenIcon } from "@radix-ui/react-icons";

export default () => (
	<PasswordToggleField.Root>
		<PasswordToggleField.Input />
		<PasswordToggleField.Toggle>
			<PasswordToggleField.Icon
				visible={<EyeOpenIcon />}
				hidden={<EyeClosedIcon />}
			/>
		</PasswordToggleField.Toggle>
	</PasswordToggleField.Root>
);
```

----------------------------------------

TITLE: Displaying Text with Color Variables in JSX
DESCRIPTION: This snippet demonstrates how to display a text element using predefined Radix Color variables within a JSX component. The Radix Color variable is used to style the text color, providing a visually distinct appearance. This implementation requires a React environment and access to Radix Colors defined as CSS variables.
SOURCE: https://github.com/radix-ui/website/blob/main/data/colors/docs/palette-composition/composing-a-palette.mdx#2025-04-21_snippet_0

LANGUAGE: JSX
CODE:
```
<Box my="5">
	<Text
		as="p"
		size="7"
		align="center"
		weight="bold"
		style={{ color: "var(--blue-12)" }}
	>
		This text is Blue 12
	</Text>
</Box>
```

----------------------------------------

TITLE: Custom Toast Duration in React
DESCRIPTION: This snippet shows how to customize the duration of a toast using the __duration__ prop on the Toast.Root component. The specified duration (3000ms) overrides the default duration set by the Toast.Provider.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/toast.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Toast.Root __duration__={3000}>
	<Toast.Description>Saved!</Toast.Description>
</Toast.Root>
```

----------------------------------------

TITLE: Abstracting Toast Parts in React
DESCRIPTION: This snippet demonstrates how to create a custom Toast component by abstracting the primitive parts from Radix UI. This allows for creating a reusable Toast component with a simplified API.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/toast.mdx#2025-04-21_snippet_10

LANGUAGE: jsx
CODE:
```
// your-toast.jsx
import { Toast as ToastPrimitive } from "radix-ui";

export const Toast = ({ title, content, children, ...props }) => {
	return (
		<ToastPrimitive.Root {...props}>
			{title && <ToastPrimitive.Title>{title}</ToastPrimitive.Title>}
			<ToastPrimitive.Description>{content}</ToastPrimitive.Description>
			{children && (
				<ToastPrimitive.Action asChild>{children}</ToastPrimitive.Action>
			)}
			<ToastPrimitive.Close aria-label="Close">
				<span aria-hidden>×</span>
			</ToastPrimitive.Close>
		</ToastPrimitive.Root>
	);
};
```

----------------------------------------

TITLE: Changing Heading Level with 'as' Prop
DESCRIPTION: This snippet shows how to change the semantic heading level using the 'as' prop, which alters the element type without affecting visual appearance.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/heading.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Heading as="h1">Level 1</Heading>
<Heading as="h2">Level 2</Heading>
<Heading as="h3">Level 3</Heading>
```

----------------------------------------

TITLE: Importing and Using One-Time Password Field Components in JSX
DESCRIPTION: This snippet demonstrates how to import and use the One-Time Password Field component from Radix UI. It sets up the main Root component with individual Input components for each character and a HiddenInput to manage the full input value. Dependencies include the radix-ui library. Ensures inputs are rendered for a complete password value representation.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/one-time-password-field.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
import { unstable_OneTimePasswordField as OneTimePasswordField } from "radix-ui";

export default () => (
	<OneTimePasswordField.Root>
		{/* one Input for each character of the value */}
		<OneTimePasswordField.Input />
		{/* single HiddenInput to store the full value */}
		<OneTimePasswordField.HiddenInput />
	</OneTimePasswordField.Root>
);
```

----------------------------------------

TITLE: Creating a Comment Popover with Radix UI in JSX
DESCRIPTION: This snippet sets up a Popover for writing comments using Radix UI components in JSX. It uses the Popover.Trigger and Popover.Content components to display a button, a comment text area, and a checkbox. Dependencies include React and Radix UI primitives, with props for styling, layout, and interactions. It outputs a styled popover with Avatar, TextArea, and Checkbox components, with limitations on styling inheritance from Radix UI.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/popover.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Popover.Root>
	<Popover.Trigger>
		<Button variant="soft">
			<ChatBubbleIcon width="16" height="16" />
			Comment
		</Button>
	</Popover.Trigger>
	<Popover.Content width="360px">
		<Flex gap="3">
			<Avatar
				size="2"
				src="https://images.unsplash.com/photo-1607346256330-dee7af15f7c5?&w=64&h=64&dpr=2&q=70&crop=focalpoint&fp-x=0.67&fp-y=0.5&fp-z=1.4&fit=crop"
				fallback="A"
				radius="full"
			/>
			<Box flexGrow="1">
				<TextArea placeholder="Write a comment…" style={{ height: 80 }} />
				<Flex gap="3" mt="3" justify="between">
					<Flex align="center" gap="2" asChild>
						<Text as="label" size="2">
							<Checkbox />
							<Text>Send to group</Text>
						</Text>
					</Flex>

					<Popover.Close>
						<Button size="1">Comment</Button>
					</Popover.Close>
				</Flex>
			</Box>
		</Flex>
	</Popover.Content>
</Popover.Root>
```

----------------------------------------

TITLE: Importing and Composing Select Component Structure in React
DESCRIPTION: This snippet demonstrates how to import and assemble the different parts of the Radix UI Select component, forming a complete functional select dropdown with trigger and content portals.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/select.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { Select } from "radix-ui";

export default () => (
	<Select.Root>
		<Select.Trigger>
			<Select.Value />
			<Select.Icon />
		</Select.Trigger>

		<Select.Portal>
			<Select.Content>
				<Select.ScrollUpButton />
				<Select.Viewport>
					<Select.Item>
						<Select.ItemText />
						<Select.ItemIndicator />
					</Select.Item>

					<Select.Group>
						<Select.Label />
						<Select.Item>
							<Select.ItemText />
							<Select.ItemIndicator />
						</Select.Item>
					</Select.Group>

					<Select.Separator />
				</Select.Viewport>
				<Select.ScrollDownButton />
				<Select.Arrow />
			</Select.Content>
			</Select.Portal>
	</Select.Root>
);
```

----------------------------------------

TITLE: Importing Individual Color CSS Files in Radix Themes
DESCRIPTION: This TypeScript snippet shows how to import individual CSS files for specific colors in Radix Themes. This approach reduces the CSS bundle size by including only the necessary color modules.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/color.mdx#2025-04-21_snippet_12

LANGUAGE: ts
CODE:
```
// Base theme tokens
import "@radix-ui/themes/tokens/base.css";

// Include just the colors you use, for example `ruby`, `teal`, and `slate`.
// Remember to import the gray tint that matches your theme setting.
import "@radix-ui/themes/tokens/colors/ruby.css";
import "@radix-ui/themes/tokens/colors/teal.css";
import "@radix-ui/themes/tokens/colors/slate.css";

// Rest of the CSS
import "@radix-ui/themes/components.css";
import "@radix-ui/themes/utilities.css";
```

----------------------------------------

TITLE: Importing Visually Hidden Component
DESCRIPTION: This code snippet shows how to import the `VisuallyHidden` component from the `@radix-ui/react-visually-hidden` package. It also demonstrates its basic usage by rendering the `VisuallyHidden.Root` component, which visually hides its children.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/utilities/visually-hidden.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
```jsx
import { VisuallyHidden } from "radix-ui";

export default () => <VisuallyHidden.Root />;
```
```

----------------------------------------

TITLE: Basic Tabs Component Structure
DESCRIPTION: Basic implementation showing the core structure of the Tabs component with its essential parts - Root, List, Trigger and Content.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/tabs.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { Tabs } from "radix-ui";

export default () => (
	<Tabs.Root>
		<Tabs.List>
			<Tabs.Trigger />
		</Tabs.List>
		<Tabs.Content />
	</Tabs.Root>
);
```

----------------------------------------

TITLE: Using Radix UI Container Component
DESCRIPTION: This code snippet demonstrates the basic usage of the Radix UI Container component to constrain content width. It wraps a DecorativeBox component within a Container with size "1", all within a Box for styling. This showcases how to nest components within the Container to manage layout and content presentation.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/container.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Box style={{ background: "var(--gray-a2)", borderRadius: "var(--radius-3)" }}>
	<Container size="1">
		<DecorativeBox>
			<Box py="9" />
		</DecorativeBox>
	</Container>
</Box>
```

----------------------------------------

TITLE: Popover with Custom Anchor (JSX)
DESCRIPTION: This example demonstrates how to use the `Popover.Anchor` component to anchor the Popover Content to a different element than the trigger. The `__asChild__` prop allows any element to be used as the anchor, enabling flexible positioning of the Popover.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/popover.mdx#2025-04-21_snippet_8

LANGUAGE: jsx
CODE:
```
"// index.jsx
import { Popover } from \"radix-ui\";
import \"./styles.css\";

export default () => (
	<Popover.Root>
		<Popover.Anchor __asChild__>
			<div className=\"Row\">
				Row as anchor <Popover.Trigger>Trigger</Popover.Trigger>
			</div>
		</Popover.Anchor>

		<Popover.Portal>
			<Popover.Content>…</Popover.Content>
		</Popover.Portal>
	</Popover.Root>
);"
```

----------------------------------------

TITLE: Composing Tooltip and Dialog Triggers in Radix UI with JSX
DESCRIPTION: Demonstrates how to compose multiple Radix UI primitives using the asChild prop. The code combines Tooltip.Trigger and Dialog.Trigger in a single custom button component, showcasing the ability to integrate multiple behaviors in one element.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/guides/composition.mdx#2025-04-21_snippet_3

LANGUAGE: JavaScript
CODE:
```
import * as React from "react";
import { Dialog, Tooltip } from "radix-ui";

const MyButton = React.forwardRef((props, forwardedRef) => (
	<button {...props} ref={forwardedRef} />
));

export default () => {
	return (
		<Dialog.Root>
			<Tooltip.Root>
				<Tooltip.Trigger asChild>
					<Dialog.Trigger asChild>
						<MyButton>Open dialog</MyButton>
					</Dialog.Trigger>
				</Tooltip.Trigger>
				<Tooltip.Portal>…</Tooltip.Portal>
			</Tooltip.Root>

			<Dialog.Portal>...</Dialog.Portal>
		</Dialog.Root>
	);
};
```

----------------------------------------

TITLE: Implementing Custom Dialog Content - JSX
DESCRIPTION: This snippet shows how to implement a custom dialog content component using Radix UI. It uses React's forwardRef to manage ref forwarding and includes a close button within the dialog content. Expected inputs include dialog content and props, while it outputs a fully functional dialog component with close functionality.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/dialog.mdx#2025-04-21_snippet_6

LANGUAGE: jsx
CODE:
```
// your-dialog.jsx
import * as React from "react";
import { Dialog as DialogPrimitive } from "radix-ui";
import { Cross1Icon } from "@radix-ui/react-icons";

export const DialogContent = React.forwardRef(
	({ children, ...props }, forwardedRef) => (
		<DialogPrimitive.Portal>
			<DialogPrimitive.Overlay />
			<DialogPrimitive.Content {...props} ref={forwardedRef}>
				{children}
				<DialogPrimitive.Close aria-label="Close">
					<Cross1Icon />
				</DialogPrimitive.Close>
			</DialogPrimitive.Content>
		</DialogPrimitive.Portal>
	),
);

export const Dialog = DialogPrimitive.Root;
export const DialogTrigger = DialogPrimitive.Trigger;
```

----------------------------------------

TITLE: Integrating Radix UI with Client-Side Routing (Next.js)
DESCRIPTION: This snippet demonstrates how to integrate the Radix UI Navigation Menu with a client-side routing library, specifically Next.js. It creates a custom `Link` component that wraps the `NextLink` component and uses `NavigationMenu.Link` to ensure accessibility and consistent keyboard control.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/navigation-menu.mdx#2025-04-21_snippet_7

LANGUAGE: jsx
CODE:
```
// index.jsx
import { usePathname } from "next/navigation";
import NextLink from "next/link";
import { NavigationMenu } from "radix-ui";
import "./styles.css";

const Link = ({ href, ...props }) => {
	const pathname = usePathname();
	const isActive = href === pathname;

	return (
		<NavigationMenu.Link asChild active={isActive}>
			<NextLink href={href} className="NavigationMenuLink" {...props} />
		</NavigationMenu.Link>
	);
};

export default () => (
	<NavigationMenu.Root>
		<NavigationMenu.List>
			<NavigationMenu.Item>
				<Link href="/">Home</Link>
			</NavigationMenu.Item>
			<NavigationMenu.Item>
				<Link href="/about">About</Link>
			</NavigationMenu.Item>
		</NavigationMenu.List>
	</NavigationMenu.Root>
);
```

----------------------------------------

TITLE: Custom Validation Function in Form
DESCRIPTION: Example of using a custom validation function with the Form.Message component.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/form.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
<Form.Field name="name">
	<Form.Label>Full name</Form.Label>
	<Form.Control />
	<Form.Message __match__={(value, formData) => value !== "John"}>
		Only John is allowed.
	</Form.Message>
</Form.Field>
```

----------------------------------------

TITLE: Initializing Select Component with Groups in React
DESCRIPTION: This snippet demonstrates the basic usage of the Select component with grouped options for fruits and vegetables. It includes disabled items and a separator between groups.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/select.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Select.Root defaultValue="apple">
	<Select.Trigger />
	<Select.Content>
		<Select.Group>
			<Select.Label>Fruits</Select.Label>
			<Select.Item value="orange">Orange</Select.Item>
			<Select.Item value="apple">Apple</Select.Item>
			<Select.Item value="grape" disabled>
				Grape
			</Select.Item>
		</Select.Group>
		<Select.Separator />
		<Select.Group>
			<Select.Label>Vegetables</Select.Label>
			<Select.Item value="carrot">Carrot</Select.Item>
			<Select.Item value="potato">Potato</Select.Item>
		</Select.Group>
	</Select.Content>
</Select.Root>
```

----------------------------------------

TITLE: Rendering Avatars in a Flex Container
DESCRIPTION: This snippet demonstrates how to render Avatar components within a Flex container, displaying both a custom image and a fallback character. It showcases the basic usage of the Avatar component.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/avatar.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Flex gap="2">
	<Avatar
		src="https://images.unsplash.com/photo-1502823403499-6ccfcf4fb453?&w=256&h=256&q=70&crop=focalpoint&fp-x=0.5&fp-y=0.3&fp-z=1&fit=crop"
		fallback="A"
	/>
	<Avatar fallback="A" />
</Flex>
```

----------------------------------------

TITLE: Basic Avatar Component Structure in React
DESCRIPTION: Basic structure showing how to import and compose the Avatar component parts together.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/avatar.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { Avatar } from "radix-ui";

export default () => (
	<Avatar.Root>
		<Avatar.Image />
		<Avatar.Fallback />
	</Avatar.Root>
);
```

----------------------------------------

TITLE: Styling Buttons with Variant Prop in JSX
DESCRIPTION: This snippet shows the use of the 'variant' prop to change the visual style of buttons in JSX using Flex layout. Five different styles ('classic', 'solid', 'soft', 'surface', 'outline') are displayed. Each variation displays a button with changed styling, driven by the 'variant' prop.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/button.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Flex align="center" gap="3">
	<Button variant="classic">Edit profile</Button>
	<Button variant="solid">Edit profile</Button>
	<Button variant="soft">Edit profile</Button>
	<Button variant="surface">Edit profile</Button>
	<Button variant="outline">Edit profile</Button>
</Flex>
```

----------------------------------------

TITLE: Toast Sensitivity with Foreground and Background Types in React
DESCRIPTION: This snippet demonstrates how to control the sensitivity of the Toast component for screen readers using the type prop. Foreground toasts are announced immediately, while background toasts are announced at the next graceful opportunity. This enables fine-grained control over the user experience for assistive technologies.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/toast.mdx#2025-04-21_snippet_7

LANGUAGE: jsx
CODE:
```
<Toast.Root type="foreground">
	<Toast.Description>File removed successfully.</Toast.Description>
	<Toast.Close>Dismiss</Toast.Close>
</Toast.Root>

<Toast.Root type="background">
	<Toast.Description>We've just released Radix 1.0.</Toast.Description>
	<Toast.Close>Dismiss</Toast.Close>
</Toast.Root>
```

----------------------------------------

TITLE: Initializing Box Component in JSX
DESCRIPTION: This code snippet demonstrates the creation of a Box component in a JSX environment, setting its width and height attributes. The Box acts as a fundamental layout building block, and the implementation requires no additional dependencies besides standard JSX support. The parameters 'width' and 'height' determine the dimensions of the Box, and its children define the content adapted within this layout block. The example includes a DecorativeBox component, suggesting customization and style adaptation.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/box.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Box width=\"64px\" height=\"64px\">\n\t<DecorativeBox />\n</Box>
```

----------------------------------------

TITLE: Basic Text Field with Search Icon
DESCRIPTION: Implementation of a basic text field with a search icon in the slot
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/text-field.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<TextField.Root placeholder="Search the docs…">
	<TextField.Slot>
		<MagnifyingGlassIcon height="16" width="16" />
	</TextField.Slot>
</TextField.Root>
```

----------------------------------------

TITLE: Integrating Icons in Buttons with JSX
DESCRIPTION: This snippet shows how to integrate icons within Button components using JSX. It demonstrates using BookmarkIcon with buttons in 'classic', 'solid', 'soft', 'surface', and 'outline' variants. Each button uses Flex for layout management, preserving the JSX format.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/button.mdx#2025-04-21_snippet_7

LANGUAGE: jsx
CODE:
```
<Flex gap="3">
	<Button variant="classic">
		<BookmarkIcon /> Bookmark
	</Button>
	<Button variant="solid">
		<BookmarkIcon /> Bookmark
	</Button>
	<Button variant="soft">
		<BookmarkIcon /> Bookmark
	</Button>
	<Button variant="surface">
		<BookmarkIcon /> Bookmark
	</Button>
	<Button variant="outline">
		<BookmarkIcon /> Bookmark
	</Button>
</Flex>
```

----------------------------------------

TITLE: Styling Links in Radix UI with Client-Side Routing
DESCRIPTION: This CSS snippet provides basic styling for the custom `NavigationMenuLink` component used in the client-side routing example. It removes the default text decoration and adds an underline when the link is active.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/navigation-menu.mdx#2025-04-21_snippet_8

LANGUAGE: css
CODE:
```
/* styles.css */
.NavigationMenuLink {
	text-decoration: none;
}
.NavigationMenuLink[data-active] {
	text-decoration: "underline";
}
```

----------------------------------------

TITLE: Applying Reset Component to HTML Elements in React
DESCRIPTION: Shows how to use the Reset component to remove default browser styles from HTML elements and set idiomatic layout defaults for custom component development.
SOURCE: https://github.com/radix-ui/website/blob/main/data/blog/themes-3.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Reset>
	<button>Button</button>
</Reset>
```

----------------------------------------

TITLE: Configuring Height on Box Component in JSX
DESCRIPTION: Illustrates how to set height on the Box component using fixed values and responsive object notation.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/overview/layout.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Box height="100px" />
<Box height={{ md: '100vh', xl: '600px' }} />
```

----------------------------------------

TITLE: Creating a Basic Hover Card with Trigger and Content in React
DESCRIPTION: This snippet demonstrates how to set up a basic Hover Card component using Radix UI. It includes a trigger link that, when hovered over, reveals additional content in the hover card. The Hover Card consists of an avatar image and descriptive text about the @radix_ui account.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/hover-card.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
"<Text>\n\tFollow{\" \\n\t<HoverCard.Root>\n\t\t<HoverCard.Trigger>\n\t\t\t<Link href=\"#\" href=\"https://twitter.com/radix_ui\" target=\"_blank\">\n\t\t\t\t@radix_ui\n\t\t\t</Link>\n\t\t</HoverCard.Trigger>\n\t\t<HoverCard.Content maxWidth=\"300px\">\n\t\t\t<Flex gap=\"4\">\n\t\t\t\t<Avatar\n\t\t\t\t\tsize=\"3\"\n\t\t\t\t\tfallback=\"R\"\n\t\t\t\t\tradius=\"full\"\n\t\t\t\t\tsrc=\"https://pbs.twimg.com/profile_images/1337055608613253126/r_eiMp2H_400x400.png\"\n\t\t\t\t/>\n\t\t\t\t<Box>\n\t\t\t\t\t<Heading size=\"3\" as=\"h3\">\n\t\t\t\t\t\tRadix\n\t\t\t\t\t</Heading>\n\t\t\t\t\t\t<Text as=\"div\" size=\"2\" color=\"gray\" mb=\"2\">\n\t\t\t\t\t\t\t@radix_ui\n\t\t\t\t\t\t</Text>\n\t\t\t\t\t\t<Text as=\"div\" size=\"2\">\n\t\t\t\t\t\t\tReact components, icons, and colors for building high-quality,\n\t\t\t\t\t\t\taccessible UI.\n\t\t\t\t\t\t</Text>\n\t\t\t\t\t</Box>\n\t\t\t\t</Flex>\n\t\t\t</HoverCard.Content>\n\t\t</HoverCard.Root>{\" \\n\tfor updates.\n\t</Text>"
```

----------------------------------------

TITLE: Using Theme Scaling in Radix UI Components
DESCRIPTION: JSX example showing how to apply scaling to Radix UI components using the Theme component with a scaling prop. This affects layout density uniformly across the application.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/spacing.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Theme scaling="100%">
	<Card variant="surface">
		<Flex gap="3" align="center">
			<Avatar size="3" src={person.image} fallback={person.name} />
			<Box>
				<Text as="div" size="2" weight="bold">
					{person.name}
				</Text>
				<Text as="div" size="2" color="gray">
					Approved invoice <Link>#3461</Link>
				</Text>
			</Box>
		</Flex>
	</Card>
</Theme>
```

----------------------------------------

TITLE: Implementing a Dialog with Asynchronous Form Submission
DESCRIPTION: Example showing how to use controlled props to programmatically close a Dialog component after an asynchronous operation (like a form submission) has completed.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/dialog.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
import * as React from "react";
import { Dialog } from "radix-ui";

const wait = () => new Promise((resolve) => setTimeout(resolve, 1000));

export default () => {
	const [open, setOpen] = React.useState(false);

	return (
		<Dialog.Root __open__={open} __onOpenChange__={setOpen}>
			<Dialog.Trigger>Open</Dialog.Trigger>
			<Dialog.Portal>
				<Dialog.Overlay />
				<Dialog.Content>
					<form
						onSubmit={(event) => {
							wait().then(() => setOpen(false));
							event.preventDefault();
						}}
					>
						{/** some inputs */}
						<button type="submit">Submit</button>
					</form>
				</Dialog.Content>
			</Dialog.Portal>
		</Dialog.Root>
	);
};
```

----------------------------------------

TITLE: Composing Avatar with Tooltip in React
DESCRIPTION: Example showing how to compose the Avatar component with a Tooltip to display additional information when the avatar is hovered or focused.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/avatar.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
import { Avatar, __Tooltip__ } from "radix-ui";

export default () => (
	<Tooltip.Root>
		<Tooltip.Trigger>
			<Avatar.Root>…</Avatar.Root>
		</Tooltip.Trigger>

		<Tooltip.Content side="top">
			Tooltip content
			<Tooltip.Arrow />
		</Tooltip.Content>
	</Tooltip.Root>
);
```

----------------------------------------

TITLE: Setting Dark Mode Appearance in Radix UI Theme
DESCRIPTION: Basic example showing how to force dark mode appearance by passing the 'appearance' prop to the Theme component.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/dark-mode.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Theme appearance="dark">
	<MyApp />
</Theme>
```

----------------------------------------

TITLE: Renaming Radix UI Color Scales in CSS-in-JS
DESCRIPTION: This snippet shows how to import Radix UI color scales in a JavaScript environment and rename them in a theme object. It demonstrates mapping variables from imported color scales to more intuitive or brand-specific names, such as renaming 'slate' to 'gray' or 'violet' to 'blurple'.
SOURCE: https://github.com/radix-ui/website/blob/main/data/colors/docs/overview/aliasing.mdx#2025-04-21_snippet_5

LANGUAGE: javascript
CODE:
```
import {
	slate,
	sky,
	grass,
	violet,
	crimson
} from "@radix-ui/colors";

const theme = {
	gray1: slate.slate1,
	gray2: slate.slate2,

	blue1: sky.sky1,
	blue2: sky.sky2,

	green1: grass.grass1,
	green2: grass.grass2,

	blurple1: violet.violet1,
	blurple2: violet.violet2,

	caribbeanSunset1: crimson.crimson1,
	caribbeanSunset2: crimson.crimson2,
};
```

----------------------------------------

TITLE: Importing and Basic Usage of Slot Component
DESCRIPTION: Demonstrates how to import and use the Slot component with basic React component structure
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/utilities/slot.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { Slot } from "radix-ui";

export default () => (
	<Slot.Root>
		<div>Hello</div>
	</Slot.Root>
);
```

----------------------------------------

TITLE: Using Spinner with Conditional Child Rendering in React
DESCRIPTION: Shows how to use the Spinner component to conditionally render a child component (CubeIcon) based on a loading state. This approach preserves child dimensions to prevent layout shifts.
SOURCE: https://github.com/radix-ui/website/blob/main/data/blog/themes-3.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Spinner loading={loading}>
	<CubeIcon />
</Spinner>
```

----------------------------------------

TITLE: Constraining DropdownMenu Content Size with CSS Custom Properties
DESCRIPTION: Demonstrates how to constrain the width and height of DropdownMenu content using CSS custom properties provided by Radix UI, such as --radix-dropdown-menu-trigger-width and --radix-dropdown-menu-content-available-height.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/dropdown-menu.mdx#2025-04-21_snippet_14

LANGUAGE: jsx
CODE:
```
// index.jsx
import { DropdownMenu } from "radix-ui";
import "./styles.css";

export default () => (
	<DropdownMenu.Root>
		<DropdownMenu.Trigger>…</DropdownMenu.Trigger>
		<DropdownMenu.Portal>
			<DropdownMenu.Content __className__="DropdownMenuContent" sideOffset={5}>
				…
			</DropdownMenu.Content>
		</DropdownMenu.Portal>
	</DropdownMenu.Root>
);
```

LANGUAGE: css
CODE:
```
/* styles.css */
.DropdownMenuContent {
	width: var(__--radix-dropdown-menu-trigger-width__);
	max-height: var(__--radix-dropdown-menu-content-available-height__);
}
```

----------------------------------------

TITLE: Creating Basic Alert Dialog in React
DESCRIPTION: This snippet implements a basic Alert Dialog with a title, description, and action buttons. It uses the AlertDialog component from a UI library, featuring a trigger button and actions for canceling or confirming an action.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/alert-dialog.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<AlertDialog.Root>
	<AlertDialog.Trigger>
		<Button color="red">Revoke access</Button>
	</AlertDialog.Trigger>
	<AlertDialog.Content maxWidth="450px">
		<AlertDialog.Title>Revoke access</AlertDialog.Title>
		<AlertDialog.Description size="2">
			Are you sure? This application will no longer be accessible and any
			existing sessions will be expired.
		</AlertDialog.Description>

		<Flex gap="3" mt="4" justify="end">
			<AlertDialog.Cancel>
				<Button variant="soft" color="gray">
					Cancel
				</Button>
			</AlertDialog.Cancel>
			<AlertDialog.Action>
				<Button variant="solid" color="red">
					Revoke access
				</Button>
			</AlertDialog.Action>
		</Flex>
	</AlertDialog.Content>
</AlertDialog.Root>
```

----------------------------------------

TITLE: Creating Flex Layouts with Radix UI - JSX
DESCRIPTION: This code snippet demonstrates how to create a flex layout using the Flex component in Radix UI. It shows multiple Box components arranged in a flex container with equal spacing between them. The Flex component leverages gap and shared layout properties.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/flex.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Flex gap=\"3\">\n\t<Box width=\"64px\" height=\"64px\">\n\t\t<DecorativeBox />\n\t</Box>\n\t<Box width=\"64px\" height=\"64px\">\n\t\t<DecorativeBox />\n\t</Box>\n\t<Box width=\"64px\" height=\"64px\">\n\t\t<DecorativeBox />\n\t</Box>\n\t<Box width=\"64px\" height=\"64px\">\n\t\t<DecorativeBox />\n\t</Box>\n\t<Box width=\"64px\" height=\"64px\">\n\t\t<DecorativeBox />\n\t</Box>\n</Flex>
```

----------------------------------------

TITLE: Customizing AlertDialog Portal Container in React
DESCRIPTION: Demonstrates how to specify a custom HTML container for the AlertDialog portal using React. This customization is useful for managing dialog positioning within specific DOM elements. Dependencies: Radix UI, React.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/alert-dialog.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
export default () => {
	const [container, setContainer] = React.useState(null);
	return (
		<div>
			<AlertDialog.Root>
				<AlertDialog.Trigger />
				<AlertDialog.Portal __container__={container}>
					<AlertDialog.Overlay />
					<AlertDialog.Content>...</AlertDialog.Content>
				</AlertDialog.Portal>
			</AlertDialog.Root>

			<div ref={setContainer} />
		</div>
	);
};
```

----------------------------------------

TITLE: Implementing Context Menu Component Structure in React
DESCRIPTION: Example showing the anatomy of the Context Menu component with all possible parts structured together, including nested submenus, checkbox items, and radio groups.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/context-menu.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { ContextMenu } from "radix-ui";

export default () => (
	<ContextMenu.Root>
		<ContextMenu.Trigger />

		<ContextMenu.Portal>
			<ContextMenu.Content>
				<ContextMenu.Label />
				<ContextMenu.Item />

				<ContextMenu.Group>
					<ContextMenu.Item />
				</ContextMenu.Group>

				<ContextMenu.CheckboxItem>
					<ContextMenu.ItemIndicator />
				</ContextMenu.CheckboxItem>

				<ContextMenu.RadioGroup>
					<ContextMenu.RadioItem>
						<ContextMenu.ItemIndicator />
					</ContextMenu.RadioItem>
				</ContextMenu.RadioGroup>

				<ContextMenu.Sub>
					<ContextMenu.SubTrigger />
					<ContextMenu.Portal>
						<ContextMenu.SubContent />
					</ContextMenu.Portal>
				</ContextMenu.Sub>

				<ContextMenu.Separator />
			</ContextMenu.Content>
		</ContextMenu.Portal>
	</ContextMenu.Root>
);
```

----------------------------------------

TITLE: Disabling Options in Radix Radio Cards Component
DESCRIPTION: This example illustrates the ability to disable specific Radio Card items, rendering them non-selectable by the user. This feature is useful in scenarios where certain options should not be available under specific conditions. It requires setting up a React application and including Radix UI.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/radio-cards.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="4" maxWidth="450px">\n\t<RadioCards.Root columns="2" defaultValue="2">\n\t\t<RadioCards.Item value="1">Off</RadioCards.Item>\n\t\t<RadioCards.Item value="2">On</RadioCards.Item>\n\t</RadioCards.Root>\n\n\t<RadioCards.Root columns="2" defaultValue="2">\n\t\t<RadioCards.Item value="1" disabled>\n\t\t\tOff\n\t\t</RadioCards.Item>\n\t\t<RadioCards.Item value="2" disabled>\n\t\t\tOn\n\t\t</RadioCards.Item>\n\t</RadioCards.Root>\n</Flex>
```

----------------------------------------

TITLE: Importing and rendering ToggleGroup component
DESCRIPTION: This code snippet demonstrates how to import the `ToggleGroup` component from `radix-ui` and render a basic ToggleGroup. It initializes a `ToggleGroup.Root` and a `ToggleGroup.Item` within a React component.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/toggle-group.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { ToggleGroup } from "radix-ui";

export default () => (
	<ToggleGroup.Root>
		<ToggleGroup.Item />
	</ToggleGroup.Root>
);

```

----------------------------------------

TITLE: Creating Disabled Radio Group Items in JSX
DESCRIPTION: Shows how to use the native 'disabled' attribute to create disabled radio buttons within a Radio Group.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/radio-group.mdx#2025-04-21_snippet_6

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="2">
	<RadioGroup.Root defaultValue="2">
		<RadioGroup.Item value="1">Off</RadioGroup.Item>
		<RadioGroup.Item value="2">On</RadioGroup.Item>
	</RadioGroup.Root>

	<RadioGroup.Root defaultValue="2">
		<RadioGroup.Item value="1" disabled>
			Off
		</RadioGroup.Item>
		<RadioGroup.Item value="2" disabled>
			On
		</RadioGroup.Item>
	</RadioGroup.Root>
</Flex>
```

----------------------------------------

TITLE: Using Labels with Radix UI Select Component
DESCRIPTION: Example showing two approaches for properly labeling a Select component for accessibility: wrapping the Select in a Label component or using htmlFor to associate a Label with the Select's Trigger.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/select.mdx#2025-04-21_snippet_11

LANGUAGE: jsx
CODE:
```
import { Select, Label } from "radix-ui";

export default () => (
	<>
		<Label>
			Country
			<Select.Root>…</Select.Root>
		</Label>

		{/* or */}

		<Label htmlFor="country">Country</Label>
		<Select.Root>
			<Select.Trigger __id__="country">…</Select.Trigger>
			<Select.Portal>
				<Select.Content>…</Select.Content>
			</Select.Portal>
		</Select.Root>
	</>
);
```

----------------------------------------

TITLE: Popover Custom API Implementation (JSX)
DESCRIPTION: This example implements a custom API for the Popover component, abstracting the `Popover.Arrow` and setting a default `sideOffset` configuration. It re-exports Root and Trigger and creates a custom PopoverContent component.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/popover.mdx#2025-04-21_snippet_11

LANGUAGE: jsx
CODE:
```
"// your-popover.jsx
import * as React from \"react\";
import { Popover as PopoverPrimitive } from \"radix-ui\";

export const Popover = PopoverPrimitive.Root;
export const PopoverTrigger = PopoverPrimitive.Trigger;

export const PopoverContent = React.forwardRef(
	({ children, ...props }, forwardedRef) => (
		<PopoverPrimitive.Portal>
			<PopoverPrimitive.Content sideOffset={5} {...props} ref={forwardedRef}>
				{children}
				<PopoverPrimitive.Arrow />
			</PopoverPrimitive.Content>
		</PopoverPrimitive.Portal>
	),
);"
```

----------------------------------------

TITLE: Slot Component with Multiple Children
DESCRIPTION: Advanced usage of Slot with Slottable to handle multiple child elements and prop passing
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/utilities/slot.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
function Button({ asChild, children, leftElement, rightElement, ...props }) {
	const Comp = asChild ? Slot.Root : "button";
	return (
		<Comp {...props}>
			{leftElement}
			<Slot.Slottable>{children}</Slot.Slottable>
			{rightElement}
		</Comp>
	);
}
```

----------------------------------------

TITLE: Button with Loading State
DESCRIPTION: Example of using the Button component's built-in loading state that automatically includes a spinner.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/spinner.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Button loading>Bookmark</Button>
```

----------------------------------------

TITLE: Creating Scrollable Overlay Dialog with Radix UI - JSX
DESCRIPTION: This snippet implements a scrollable dialog using the Radix UI Dialog component. It allows overflow content to be displayed inside an overlay. The 'Dialog' component imports from Radix UI and is styled using an external CSS file. The expected input includes components wrapped in the 'Dialog' for rendering, while it outputs a functional dialog component on the screen.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/dialog.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
// index.jsx
import { Dialog } from "radix-ui";
import "./styles.css";

export default () => {
	return (
		<Dialog.Root>
			<Dialog.Trigger />
			<Dialog.Portal>
				<Dialog.Overlay className="DialogOverlay">
					<Dialog.Content className="DialogContent">...</Dialog.Content>
				</Dialog.Overlay>
			</Dialog.Portal>
		</Dialog.Root>
	);
};
```

LANGUAGE: css
CODE:
```
/* styles.css */
.DialogOverlay {
	background: rgba(0 0 0 / 0.5);
	position: fixed;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	display: grid;
	place-items: center;
	overflow-y: auto;
}

.DialogContent {
	min-width: 300px;
	background: white;
	padding: 30px;
	border-radius: 4px;
}
```

----------------------------------------

TITLE: Using Callout as an Alert in React
DESCRIPTION: Explains the integration of a WAI-ARIA `alert` role for callout components to immediately capture users' attention in critical scenarios. This feature is crucial when displaying urgent messages such as errors. Required configurations involve setting the `role` attribute to `alert`, and this technique requires Radix UI and familiarity with ARIA roles.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/callout.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
<Callout.Root color=\"red\" role=\"alert\">\n\t<Callout.Icon>\n\t\t<ExclamationTriangleIcon />\n\t</Callout.Icon>\n\t<Callout.Text>\n\t\tAccess denied. Please contact the network administrator to view this page.\n\t</Callout.Text>\n</Callout.Root>
```

----------------------------------------

TITLE: Navigation Menu Component Structure
DESCRIPTION: Example showing the complete anatomy of the Navigation Menu component with all possible parts including root, list, items, triggers, content, sub-menus, and viewport.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/navigation-menu.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { NavigationMenu } from "radix-ui";

export default () => (
	<NavigationMenu.Root>
		<NavigationMenu.List>
			<NavigationMenu.Item>
				<NavigationMenu.Trigger />
				<NavigationMenu.Content>
					<NavigationMenu.Link />
				</NavigationMenu.Content>
			</NavigationMenu.Item>

			<NavigationMenu.Item>
				<NavigationMenu.Link />
			</NavigationMenu.Item>

			<NavigationMenu.Item>
				<NavigationMenu.Trigger />
				<NavigationMenu.Content>
					<NavigationMenu.Sub>
						<NavigationMenu.List />
						<NavigationMenu.Viewport />
					</NavigationMenu.Sub>
				</NavigationMenu.Content>
			</NavigationMenu.Item>

			<NavigationMenu.Indicator />
		</NavigationMenu.List>

		<NavigationMenu.Viewport />
	</NavigationMenu.Root>
)
```

----------------------------------------

TITLE: Creating Collision-Aware Animations for Context Menu in Radix UI
DESCRIPTION: Example of using data-side and data-align attributes to create animations that adapt based on collision direction, with different animations for top and bottom collisions.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/context-menu.mdx#2025-04-21_snippet_20

LANGUAGE: jsx
CODE:
```
// index.jsx
import { ContextMenu } from "radix-ui";
import "./styles.css";

export default () => (
	<ContextMenu.Root>
		<ContextMenu.Trigger>…</ContextMenu.Trigger>
		<ContextMenu.Portal>
			<ContextMenu.Content __className__="ContextMenuContent">
				…
			</ContextMenu.Content>
		</ContextMenu.Portal>
	</ContextMenu.Root>
);
```

LANGUAGE: css
CODE:
```
/* styles.css */
.ContextMenuContent {
	animation-duration: 0.6s;
	animation-timing-function: cubic-bezier(0.16, 1, 0.3, 1);
}
.ContextMenuContent[__data-side="top"__] {
	animation-name: slideUp;
}
.ContextMenuContent[__data-side="bottom"__] {
	animation-name: slideDown;
}

@keyframes slideUp {
	from {
		opacity: 0;
		transform: translateY(10px);
	}
	to {
		opacity: 1;
		transform: translateY(0);
	}
}

@keyframes slideDown {
	from {
		opacity: 0;
		transform: translateY(-10px);
	}
	to {
		opacity: 1;
		transform: translateY(0);
	}
}
```

----------------------------------------

TITLE: Composing Form Control with Select Element
DESCRIPTION: Shows how to use the asChild prop to compose a Form Control with a select element.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/form.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Form.Field name="country">
	<Form.Label>Country</Form.Label>
	<Form.Control __asChild__>
		<select>
			<option value="uk">United Kingdom</option>…
		</select>
	</Form.Control>
</Form.Field>
```

----------------------------------------

TITLE: Setting Theme Gray Color in React
DESCRIPTION: Demonstrates how to set a custom gray color on the Theme component
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/color.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Theme grayColor="mauve">
	<MyApp />
</Theme>
```

----------------------------------------

TITLE: Implementing Radio Cards with Default Settings in React
DESCRIPTION: This snippet demonstrates a basic implementation of a Radio Cards component in React using the Radix UI library. It sets up a set of cards that allows a user to select one at a time. The default card selected is controlled by the "defaultValue" prop, and the layout of the cards is responsive, adjusting from one column to three based on screen size. Dependencies include the Radix UI library and a React environment.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/radio-cards.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Box maxWidth="600px">\n\t<RadioCards.Root defaultValue="1" columns={{ initial: "1", sm: "3" }}>\n\t\t<RadioCards.Item value="1">\n\t\t\t<Flex direction="column" width="100%">\n\t\t\t\t<Text weight="bold">8-core CPU</Text>\n\t\t\t\t<Text>32 GB RAM</Text>\n\t\t\t</Flex>\n\t\t</RadioCards.Item>\n\t\t<RadioCards.Item value="2">\n\t\t\t<Flex direction="column" width="100%">\n\t\t\t\t<Text weight="bold">6-core CPU</Text>\n\t\t\t\t<Text>24 GB RAM</Text>\n\t\t\t</Flex>\n\t\t</RadioCards.Item>\n\t\t<RadioCards.Item value="3">\n\t\t\t<Flex direction="column" width="100%">\n\t\t\t\t<Text weight="bold">4-core CPU</Text>\n\t\t\t\t<Text>16 GB RAM</Text>\n\t\t\t</Flex>\n\t\t</RadioCards.Item>\n\t</RadioCards.Root>\n</Box>
```

----------------------------------------

TITLE: Implementing DropdownMenu with Submenus in React
DESCRIPTION: This snippet demonstrates how to create a DropdownMenu with submenus using the DropdownMenu.Sub component and its related parts.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/dropdown-menu.mdx#2025-04-21_snippet_7

LANGUAGE: jsx
CODE:
```
<DropdownMenu.Root>
	<DropdownMenu.Trigger>…</DropdownMenu.Trigger>
	<DropdownMenu.Portal>
		<DropdownMenu.Content>
			<DropdownMenu.Item>…</DropdownMenu.Item>
			<DropdownMenu.Item>…</DropdownMenu.Item>
			<DropdownMenu.Separator />
			<DropdownMenu.Sub>
				<DropdownMenu.SubTrigger>Sub menu →</DropdownMenu.SubTrigger>
				<DropdownMenu.Portal>
					<DropdownMenu.SubContent>
						<DropdownMenu.Item>Sub menu item</DropdownMenu.Item>
						<DropdownMenu.Item>Sub menu item</DropdownMenu.Item>
						<DropdownMenu.Arrow />
					</DropdownMenu.SubContent>
				</DropdownMenu.Portal>
			</DropdownMenu.Sub>
			<DropdownMenu.Separator />
			<DropdownMenu.Item>…</DropdownMenu.Item>
		</DropdownMenu.Content>
	</DropdownMenu.Portal>
</DropdownMenu.Root>
```

----------------------------------------

TITLE: React Spring Animation with Radix Dialog Component
DESCRIPTION: Demonstrates using React Spring and forceMount prop to create advanced animations for Dialog components with custom enter and exit transitions.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/guides/animation.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { Dialog } from "radix-ui";
import { useTransition, animated, config } from "react-spring";

function Example() {
	const [open, setOpen] = React.useState(false);
	const transitions = useTransition(open, {
		from: { opacity: 0, y: -10 },
		enter: { opacity: 1, y: 0 },
		leave: { opacity: 0, y: 10 },
		config: config.stiff,
	});
	return (
		<Dialog.Root open={open} onOpenChange={setOpen}>
			<Dialog.Trigger>Open Dialog</Dialog.Trigger>
			{transitions((styles, item) =>
				item ? (
					<>
						<Dialog.Overlay forceMount asChild>
							<animated.div
								style={{
									opacity: styles.opacity,
								}}
							/>
						</Dialog.Overlay>
						<Dialog.Content forceMount asChild>
							<animated.div style={styles}>
								<h1>Hello from inside the Dialog!</h1>
								<Dialog.Close>close</Dialog.Close>
							</animated.div>
						</Dialog.Content>
					</>
				) : null,
			)}
		</Dialog.Root>
	);
}
```

----------------------------------------

TITLE: Composing AlertDialog in Radix UI with React
DESCRIPTION: This React component demonstrates how to construct an AlertDialog using the Radix UI library. This includes importing necessary parts from Radix UI and rendering them in a structured manner. Dependencies: Radix UI, React.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/alert-dialog.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { AlertDialog } from "radix-ui";

export default () => (
	<AlertDialog.Root>
		<AlertDialog.Trigger />
		<AlertDialog.Portal>
			<AlertDialog.Overlay />
			<AlertDialog.Content>
				<AlertDialog.Title />
				<AlertDialog.Description />
				<AlertDialog.Cancel />
				<AlertDialog.Action />
			</AlertDialog.Content>
		</AlertDialog.Portal>
	</AlertDialog.Root>
);
```

----------------------------------------

TITLE: Toolbar Component Anatomy
DESCRIPTION: This code snippet demonstrates the basic anatomy of a Radix UI Toolbar component in React. It shows how to import the Toolbar components and create a simple toolbar with a button, separator, link, and toggle group.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/toolbar.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { Toolbar } from "radix-ui";

export default () => (
	<Toolbar.Root>
		<Toolbar.Button />
		<Toolbar.Separator />
		<Toolbar.Link />
		<Toolbar.ToggleGroup>
			<Toolbar.ToggleItem />
		</Toolbar.ToggleGroup>
	</Toolbar.Root>
);
```

----------------------------------------

TITLE: Implementing DropdownMenu with Complex Items in React
DESCRIPTION: Shows how to add decorative elements like images to dropdown menu items. The example demonstrates creating menu items with images alongside text content.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/dropdown-menu.mdx#2025-04-21_snippet_13

LANGUAGE: jsx
CODE:
```
import { DropdownMenu } from "radix-ui";

export default () => (
	<DropdownMenu.Root>
		<DropdownMenu.Trigger>…</DropdownMenu.Trigger>
		<DropdownMenu.Portal>
			<DropdownMenu.Content>
				<DropdownMenu.Item>
					<img src="…" />
					Adolfo Hess
				</DropdownMenu.Item>
				<DropdownMenu.Item>
					<img src="…" />
					Miyah Myles
				</DropdownMenu.Item>
			</DropdownMenu.Content>
		</DropdownMenu.Portal>
	</DropdownMenu.Root>
);
```

----------------------------------------

TITLE: Using Radix Themes Components in React
DESCRIPTION: Example of how to use Radix Themes components (Flex, Text, Button) in a React application.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/overview/getting-started.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
import { Flex, Text, Button } from "@radix-ui/themes";

export default function MyApp() {
	return (
		<Flex direction="column" gap="2">
			<Text>Hello from Radix Themes :)</Text>
			<Button>Let's go</Button>
		</Flex>
	);
}
```

----------------------------------------

TITLE: Styling Radix UI Accordion Item state with styled-components
DESCRIPTION: This snippet illustrates how to style a Radix UI Accordion Item's state using styled-components and the `data-state` attribute. It uses the `&` selector to target the `data-state` attribute and apply specific styles when the item is open.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/guides/styling.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
import { Accordion } from "radix-ui";
import styled from "styled-components";

const StyledItem = styled(Accordion.Item)`
	border-bottom: 1px solid gainsboro;

	&[__data-state__="open"] {
		border-bottom-width: 2px;
	}
`;
```

----------------------------------------

TITLE: Adding Separators to ContextMenu in React
DESCRIPTION: Example demonstrating how to add separators between items in a ContextMenu to visually separate groups of menu items.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/context-menu.mdx#2025-04-21_snippet_13

LANGUAGE: jsx
CODE:
```
<ContextMenu.Root>
	<ContextMenu.Trigger>…</ContextMenu.Trigger>
	<ContextMenu.Portal>
		<ContextMenu.Content>
			<ContextMenu.Item>…</ContextMenu.Item>
			<ContextMenu.Separator />
			<ContextMenu.Item>…</ContextMenu.Item>
			<ContextMenu.Separator />
			<ContextMenu.Item>…</ContextMenu.Item>
		</ContextMenu.Content>
	</ContextMenu.Portal>
</ContextMenu.Root>
```

----------------------------------------

TITLE: Alternative Action for Toasts in React
DESCRIPTION: This snippet shows how to use the altText prop on the Toast.Action component to provide an alternative way for screen reader users to achieve the action associated with the toast. It also demonstrates setting the type and duration props to improve accessibility and usability.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/toast.mdx#2025-04-21_snippet_8

LANGUAGE: jsx
CODE:
```
<Toast.Root type="background">
	<Toast.Title>Upgrade Available!</Toast.Title>
	<Toast.Description>We've just released Radix 1.0.</Toast.Description>
	<Toast.Action altText="Goto account settings to upgrade">
		Upgrade
	</Toast.Action>
	<Toast.Close>Dismiss</Toast.Close>
</Toast.Root>

<Toast.Root __type__="foreground" __duration__={10000}>
	<Toast.Description>File removed successfully.</Toast.Description>
	<Toast.Action altText="Undo (Alt+U)">
		Undo <kbd>Alt</kbd>+<kbd>U</kbd>
	</Toast.Action>
	<Toast.Close>Dismiss</Toast.Close>
</Toast.Root>
```

----------------------------------------

TITLE: Installing Radix UI Separator Component
DESCRIPTION: Command to install the Separator component from the Radix UI library using npm.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/separator.mdx#2025-04-21_snippet_0

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-separator
```

----------------------------------------

TITLE: Installing Tooltip Component with npm
DESCRIPTION: This snippet illustrates how to install the Radix UI tooltip component using npm. It ensures users have the necessary package installed in their project to utilize tooltips effectively.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/tooltip.mdx#2025-04-21_snippet_0

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-tooltip
```

----------------------------------------

TITLE: Installing Radix UI Slider Component
DESCRIPTION: Command to install the Radix UI Slider component package from npm.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/slider.mdx#2025-04-21_snippet_0

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-slider
```

----------------------------------------

TITLE: Using Flex Children Props on Box Component in JSX
DESCRIPTION: Shows how to use flex children props on the Box component to control its behavior within a flex container.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/overview/layout.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
<Box flexBasis="100%" />
<Box flexShrink="0">
<Box flexGrow={{ initial: "0", lg: "1" }} />
```

----------------------------------------

TITLE: Popover with Content, Trigger, and CSS
DESCRIPTION: This JSX snippet shows the basic structure of a Popover component with `Popover.Root`, `Popover.Trigger`, `Popover.Portal`, and `Popover.Content`. It also links to a CSS file for styling, demonstrating how to integrate the Popover component into a React application.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/popover.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
"// index.jsx
import { Popover } from \"radix-ui\";
import \"./styles.css\";

export default () => (
	<Popover.Root>
		<Popover.Trigger>…</Popover.Trigger>
		<Popover.Portal>
			<Popover.Content __className__=\"PopoverContent\">…</Popover.Content>
		</Popover.Portal>
	</Popover.Root>
);"
```

----------------------------------------

TITLE: Accessing Form Validity State Programmatically
DESCRIPTION: Shows how to access the raw validity state of a form field for custom handling or integration with component libraries
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/form.mdx#2025-04-21_snippet_7

LANGUAGE: jsx
CODE:
```
<Form.Field name="name">
	<Form.Label>Full name</Form.Label>
	<Form.ValidityState>
		{(validity) => (
			<Form.Control asChild>
				<TextField.Input
					variant="primary"
					state={getTextFieldInputState(__validity__)}
				/>
			</Form.Control>
		)}
	</Form.ValidityState>
</Form.Field>
```

----------------------------------------

TITLE: Text Component with Different Colors in JSX
DESCRIPTION: Example showing how to use the 'color' prop to assign specific colors to text. The component's colors are designed to achieve sufficient contrast over common background colors.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/text.mdx#2025-04-21_snippet_14

LANGUAGE: jsx
CODE:
```
<Flex direction="column">
	<Text color="indigo">The quick brown fox jumps over the lazy dog.</Text>
	<Text color="cyan">The quick brown fox jumps over the lazy dog.</Text>
	<Text color="orange">The quick brown fox jumps over the lazy dog.</Text>
	<Text color="crimson">The quick brown fox jumps over the lazy dog.</Text>
</Flex>
```

----------------------------------------

TITLE: Customizing Select Trigger with Icons in React
DESCRIPTION: This snippet demonstrates how to customize the Select Trigger by manually controlling its children. It shows how to render an icon next to the selected item's text.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/select.mdx#2025-04-21_snippet_10

LANGUAGE: jsx
CODE:
```
() => {
	const data = {
		light: { label: "Light", icon: <SunIcon /> },
		dark: { label: "Dark", icon: <MoonIcon /> },
	};
	const [value, setValue] = React.useState("light");
	return (
		<Flex direction="column" maxWidth="160px">
			<Select.Root value={value} onValueChange={setValue}>
				<Select.Trigger>
					<Flex as="span" align="center" gap="2">
						{data[value].icon}
						{data[value].label}
					</Flex>
				</Select.Trigger>
				<Select.Content position="popper">
					<Select.Item value="light">Light</Select.Item>
					<Select.Item value="dark">Dark</Select.Item>
				</Select.Content>
			</Select.Root>
		</Flex>
	);
};
```

----------------------------------------

TITLE: Using Multiple Semantic Aliases for the Same Color Scale in CSS
DESCRIPTION: This snippet demonstrates how to handle situations where multiple semantic terms need to map to the same color scale. It shows how to define multiple semantic aliases (like 'info'/'accent', 'valid'/'success', 'pending'/'warning') for the same color values using CSS custom properties.
SOURCE: https://github.com/radix-ui/website/blob/main/data/colors/docs/overview/aliasing.mdx#2025-04-21_snippet_2

LANGUAGE: css
CODE:
```
/*
 * Note: Importing from the CDN in production is not recommended.
 * It's intended for prototyping only.
 */
@import "https://cdn.jsdelivr.net/npm/@radix-ui/colors@latest/blue.css";
@import "https://cdn.jsdelivr.net/npm/@radix-ui/colors@latest/green.css";
@import "https://cdn.jsdelivr.net/npm/@radix-ui/colors@latest/yellow.css";

:root {
	--accent-1: var(--blue-1);
	--accent-2: var(--blue-2);
	--info-1: var(--blue-1);
	--info-2: var(--blue-2);

	--success-1: var(--green-1);
	--success-2: var(--green-2);
	--valid-1: var(--green-1);
	--valid-2: var(--green-2);

	--warning-1: var(--yellow-1);
	--warning-2: var(--yellow-2);
	--pending-1: var(--yellow-1);
	--pending-2: var(--yellow-2);
}
```

----------------------------------------

TITLE: Text Field with Complex Composition
DESCRIPTION: Advanced implementation showing text fields with multiple slots and nested interactive elements
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/text-field.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="3" maxWidth="400px">
	<Box maxWidth="200px">
		<TextField.Root placeholder="Search the docs…" size="1">
			<TextField.Slot>
				<MagnifyingGlassIcon height="16" width="16" />
			</TextField.Slot>
		</TextField.Root>
	</Box>

	<Box maxWidth="250px">
		<TextField.Root placeholder="Search the docs…" size="2">
			<TextField.Slot>
				<MagnifyingGlassIcon height="16" width="16" />
			</TextField.Slot>
			<TextField.Slot>
				<IconButton size="1" variant="ghost">
					<DotsHorizontalIcon height="14" width="14" />
				</IconButton>
			</TextField.Slot>
		</TextField.Root>
	</Box>

	<Box maxWidth="300px">
		<TextField.Root placeholder="Search the docs…" size="3">
			<TextField.Slot>
				<MagnifyingGlassIcon height="16" width="16" />
			</TextField.Slot>
			<TextField.Slot pr="3">
				<IconButton size="2" variant="ghost">
					<DotsHorizontalIcon height="16" width="16" />
				</IconButton>
			</TextField.Slot>
		</TextField.Root>
	</Box>
</Flex>
```

----------------------------------------

TITLE: Enhancing Contrast for Buttons in JSX
DESCRIPTION: This code snippet demonstrates the use of the 'highContrast' prop to improve button visibility against backgrounds. It shows grey-colored buttons with and without high contrast, provided in 'classic', 'solid', 'soft', 'surface', and 'outline' variants.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/button.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="3">
	<Flex gap="3">
		<Button color="gray" variant="classic">
			Edit profile
		</Button>
		<Button color="gray" variant="solid">
			Edit profile
		</Button>
		<Button color="gray" variant="soft">
			Edit profile
		</Button>
		<Button color="gray" variant="surface">
			Edit profile
		</Button>
		<Button color="gray" variant="outline">
			Edit profile
		</Button>
	</Flex>
	<Flex gap="3">
		<Button color="gray" variant="classic" highContrast>
			Edit profile
		</Button>
		<Button color="gray" variant="solid" highContrast>
			Edit profile
		</Button>
		<Button color="gray" variant="soft" highContrast>
			Edit profile
		</Button>
		<Button color="gray" variant="surface" highContrast>
			Edit profile
		</Button>
		<Button color="gray" variant="outline" highContrast>
			Edit profile
		</Button>
	</Flex>
</Flex>
```

----------------------------------------

TITLE: Text Field Visual Variants
DESCRIPTION: Demonstration of different visual styles using the variant prop
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/text-field.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="3" maxWidth="250px">
	<TextField.Root variant="surface" placeholder="Search the docs…" />
	<TextField.Root variant="classic" placeholder="Search the docs…" />
	<TextField.Root variant="soft" placeholder="Search the docs…" />
</Flex>
```

----------------------------------------

TITLE: Adjust DataList Size in Radix UI with JSX
DESCRIPTION: Demonstrates changing the size of the DataList using the 'size' prop, applying different size values ('1', '2', '3') to illustrate the component's scalability within a column layout. Radix UI and Flex components are utilized to manage styling and layout. Import these from the Radix UI library.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/data-list.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="6">
	<DataList.Root size="1">
		<DataList.Item>
			<DataList.Label minWidth="88px">Name</DataList.Label>
			<DataList.Value>Vlad Moroz</DataList.Value>
		</DataList.Item>
		<DataList.Item>
			<DataList.Label minWidth="88px">Email</DataList.Label>
			<DataList.Value>
				<Link href="mailto:vlad@workos.com">vlad@workos.com</Link>
			</DataList.Value>
		</DataList.Item>
		<DataList.Item>
			<DataList.Label minWidth="88px">Company</DataList.Label>
			<DataList.Value>
				<Link target="_blank" href="https://workos.com">
					WorkOS
				</Link>
			</DataList.Value>
		</DataList.Item>
	</DataList.Root>

	<DataList.Root size="2">
		<DataList.Item>
			<DataList.Label minWidth="88px">Name</DataList.Label>
			<DataList.Value>Vlad Moroz</DataList.Value>
		</DataList.Item>
		<DataList.Item>
			<DataList.Label minWidth="88px">Email</DataList.Label>
			<DataList.Value>
				<Link href="mailto:vlad@workos.com">vlad@workos.com</Link>
			</DataList.Value>
		</DataList.Item>
		<DataList.Item>
			<DataList.Label minWidth="88px">Company</DataList.Label>
			<DataList.Value>
				<Link target="_blank" href="https://workos.com">
					WorkOS
				</Link>
			</DataList.Value>
		</DataList.Item>
	</DataList.Root>

	<DataList.Root size="3">
		<DataList.Item>
			<DataList.Label minWidth="88px">Name</DataList.Label>
			<DataList.Value>Vlad Moroz</DataList.Value>
		</DataList.Item>
		<DataList.Item>
			<DataList.Label minWidth="88px">Email</DataList.Label>
			<DataList.Value>
				<Link href="mailto:vlad@workos.com">vlad@workos.com</Link>
			</DataList.Value>
		</DataList.Item>
		<DataList.Item>
			<DataList.Label minWidth="88px">Company</DataList.Label>
			<DataList.Value>
				<Link target="_blank" href="https://workos.com">
					WorkOS
				</Link>
			</DataList.Value>
		</DataList.Item>
	</DataList.Root>
</Flex>
```

----------------------------------------

TITLE: Text Field Size Variants
DESCRIPTION: Examples of text fields with different size properties showing responsive scaling
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/text-field.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="3">
	<Box maxWidth="200px">
		<TextField.Root size="1" placeholder="Search the docs…" />
	</Box>
	<Box maxWidth="250px">
		<TextField.Root size="2" placeholder="Search the docs…" />
	</Box>
	<Box maxWidth="300px">
		<TextField.Root size="3" placeholder="Search the docs…" />
	</Box>
</Flex>
```

----------------------------------------

TITLE: Slider Size Variations in React
DESCRIPTION: Demonstrates using the 'size' prop to control the Slider component's size with three different values.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/slider.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="4" maxWidth="300px">
	<Slider defaultValue={[25]} size="1" />
	<Slider defaultValue={[50]} size="2" />
	<Slider defaultValue={[75]} size="3" />
</Flex>
```

----------------------------------------

TITLE: Customizing Dropdown Menu Style with Variant Prop
DESCRIPTION: This snippet illustrates how to use the `variant` prop to change the visual style of the dropdown menu and its items. It provides examples of a solid and soft variant applied to the trigger button and the content.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/dropdown-menu.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Flex gap="3" align="center">
	<DropdownMenu.Root>
		<DropdownMenu.Trigger>
			<Button variant="solid">
				Options
				<DropdownMenu.TriggerIcon />
			</Button>
		</DropdownMenu.Trigger>
		<DropdownMenu.Content variant="solid">
			<DropdownMenu.Item shortcut="⌘ E">Edit</DropdownMenu.Item>
			<DropdownMenu.Item shortcut="⌘ D">Duplicate</DropdownMenu.Item>
			<DropdownMenu.Separator />
			<DropdownMenu.Item shortcut="⌘ N">Archive</DropdownMenu.Item>

			<DropdownMenu.Separator />
			<DropdownMenu.Item shortcut="⌘ ⌫" color="red">
				Delete
			</DropdownMenu.Item>
		</DropdownMenu.Content>
	</DropdownMenu.Root>

	<DropdownMenu.Root>
		<DropdownMenu.Trigger>
			<Button variant="soft">
				Options
				<DropdownMenu.TriggerIcon />
			</Button>
		</DropdownMenu.Trigger>
		<DropdownMenu.Content variant="soft">
			<DropdownMenu.Item shortcut="⌘ E">Edit</DropdownMenu.Item>
			<DropdownMenu.Item shortcut="⌘ D">Duplicate</DropdownMenu.Item>
			<DropdownMenu.Separator />
			<DropdownMenu.Item shortcut="⌘ N">Archive</DropdownMenu.Item>

			<DropdownMenu.Separator />
			<DropdownMenu.Item shortcut="⌘ ⌫" color="red">
				Delete
			</DropdownMenu.Item>
		</DropdownMenu.Content>
	</DropdownMenu.Root>
</Flex>
```

----------------------------------------

TITLE: Custom Brand Color CSS Override
DESCRIPTION: Shows how to override theme colors with custom brand colors
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/color.mdx#2025-04-21_snippet_10

LANGUAGE: css
CODE:
```
.radix-themes {
	--my-brand-color: #3052f6;
	--indigo-9: var(--my-brand-color);
	--indigo-a9: var(--my-brand-color);
}
```

----------------------------------------

TITLE: Animating Popover Content with CSS
DESCRIPTION: This CSS snippet demonstrates how to apply an origin-aware animation to the Popover.Content component using the `--radix-popover-content-transform-origin` CSS custom property.  It defines a scale-in animation that starts from opacity 0 and scale 0, transitioning to opacity 1 and scale 1 over 0.5 seconds with an ease-out timing function.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/popover.mdx#2025-04-21_snippet_5

LANGUAGE: css
CODE:
```
"/* styles.css */
.PopoverContent {
	transform-origin: var(__--radix-popover-content-transform-origin__);
	animation: scaleIn 0.5s ease-out;
}

@keyframes scaleIn {
	from {
		opacity: 0;
		transform: scale(0);
	}
	to {
		opacity: 1;
		transform: scale(1);
	}
}"
```

----------------------------------------

TITLE: Implementing Indeterminate Checkbox State
DESCRIPTION: Example implementation of a checkbox with indeterminate state support using React state management.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/checkbox.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
import { DividerHorizontalIcon, CheckIcon } from "@radix-ui/react-icons";
import { Checkbox } from "radix-ui";

export default () => {
	const [checked, setChecked] = React.useState("indeterminate");

	return (
		<>
			<StyledCheckbox checked={checked} onCheckedChange={setChecked}>
				<Checkbox.Indicator>
					{checked === "indeterminate" && <DividerHorizontalIcon />}
					{checked === true && <CheckIcon />}
				</Checkbox.Indicator>
			</StyledCheckbox>

			<button
				type="button"
				onClick={() =>
					setChecked((prevIsChecked) =>
						prevIsChecked === "indeterminate" ? false : "indeterminate",
					)
				}
			>
				Toggle indeterminate
			</button>
		</>
	);
};
```

----------------------------------------

TITLE: Displaying Data List with Radix UI in JSX
DESCRIPTION: Demonstrates the use of Radix UI's DataList component to show metadata as a list of key-value pairs. The snippet includes labels for 'Status', 'ID', 'Name', 'Email', and 'Company'. It utilizes the Badge, Flex, Code, IconButton, and Link components for layout and interactivity. Dependencies include the Radix UI component library.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/data-list.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<DataList.Root>
	<DataList.Item align="center">
		<DataList.Label minWidth="88px">Status</DataList.Label>
		<DataList.Value>
			<Badge color="jade" variant="soft" radius="full">
				Authorized
			</Badge>
		</DataList.Value>
	</DataList.Item>
	<DataList.Item>
		<DataList.Label minWidth="88px">ID</DataList.Label>
		<DataList.Value>
			<Flex align="center" gap="2">
				<Code variant="ghost">u_2J89JSA4GJ</Code>
				<IconButton
					size="1"
					aria-label="Copy value"
					color="gray"
					variant="ghost"
				>
					<CopyIcon />
				</IconButton>
			</Flex>
		</DataList.Value>
	</DataList.Item>
	<DataList.Item>
		<DataList.Label minWidth="88px">Name</DataList.Label>
		<DataList.Value>Vlad Moroz</DataList.Value>
	</DataList.Item>
	<DataList.Item>
		<DataList.Label minWidth="88px">Email</DataList.Label>
		<DataList.Value>
			<Link href="mailto:vlad@workos.com">vlad@workos.com</Link>
		</DataList.Value>
	</DataList.Item>
	<DataList.Item>
		<DataList.Label minWidth="88px">Company</DataList.Label>
		<DataList.Value>
			<Link target="_blank" href="https://workos.com">
				WorkOS
			</Link>
		</DataList.Value>
	</DataList.Item>
</DataList.Root>
```

----------------------------------------

TITLE: Switch Component with Different Sizes in JSX
DESCRIPTION: Demonstrates different sizes of the Switch component using the size prop with values 1, 2, and 3, all with defaultChecked set to true.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/switch.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Flex align="center" gap="2">
	<Switch size="1" defaultChecked />
	<Switch size="2" defaultChecked />
	<Switch size="3" defaultChecked />
</Flex>
```

----------------------------------------

TITLE: High Contrast Checkboxes in JSX
DESCRIPTION: Shows how to increase contrast using the highContrast prop on Checkbox components. Displays a grid layout with checkboxes of varying colors and high-contrast settings enabled. This showcases enhancing visibility against different backgrounds.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/checkbox.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
<Grid columns="5" display="inline-grid" gap="2">
	<Checkbox color="indigo" defaultChecked />
	<Checkbox color="cyan" defaultChecked />
	<Checkbox color="orange" defaultChecked />
	<Checkbox color="crimson" defaultChecked />
	<Checkbox color="gray" defaultChecked />

	<Checkbox color="indigo" defaultChecked highContrast />
	<Checkbox color="cyan" defaultChecked highContrast />
	<Checkbox color="orange" defaultChecked highContrast />
	<Checkbox color="crimson" defaultChecked highContrast />
	<Checkbox color="gray" defaultChecked highContrast />
</Grid>
```

----------------------------------------

TITLE: Slider with Custom Colors in React
DESCRIPTION: Demonstrates applying different colors to the Slider component using the 'color' prop.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/slider.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="4" maxWidth="300px">
	<Slider defaultValue={[20]} color="indigo" />
	<Slider defaultValue={[40]} color="cyan" />
	<Slider defaultValue={[60]} color="orange" />
	<Slider defaultValue={[80]} color="crimson" />
</Flex>
```

----------------------------------------

TITLE: CSS for Constraining HoverCard Content Size
DESCRIPTION: This CSS snippet defines styles for the 'HoverCardContent' class, constraining its width to the width of the trigger element using the `--radix-hover-card-trigger-width` CSS custom property and setting a maximum height based on the available height with `--radix-hover-card-content-available-height`. This allows the content dimensions to adapt dynamically to the trigger size and available space.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/hover-card.mdx#2025-04-21_snippet_4

LANGUAGE: css
CODE:
```
.HoverCardContent {
	width: var(__--radix-hover-card-trigger-width__);
	max-height: var(__--radix-hover-card-content-available-height__);
}
```

----------------------------------------

TITLE: Combining Spinner with Icon in Buttons in JSX
DESCRIPTION: This snippet demonstrates how to combine disabled state and Spinner to create more sophisticated button designs in JSX, where the spinner wraps icons for improved visuals. It showcases buttons across variants, integrating BookmarkIcon with Spinner.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/button.mdx#2025-04-21_snippet_9

LANGUAGE: jsx
CODE:
```
<Flex gap="3">
	<Button disabled variant="classic">
		<Spinner loading>
			<BookmarkIcon />
		</Spinner>
		Bookmark
	</Button>
	<Button disabled variant="solid">
		<Spinner loading>
			<BookmarkIcon />
		</Spinner>
		Bookmark
	</Button>
	<Button disabled variant="soft">
		<Spinner loading>
			<BookmarkIcon />
		</Spinner>
		Bookmark
	</Button>
	<Button disabled variant="surface">
		<Spinner loading>
			<BookmarkIcon />
		</Spinner>
		Bookmark
	</Button>
	<Button disabled variant="outline">
		<Spinner loading>
			<BookmarkIcon />
		</Spinner>
		Bookmark
	</Button>
</Flex>
```

----------------------------------------

TITLE: Initializing Segmented Control Component in JSX
DESCRIPTION: This snippet demonstrates the basic usage of the Segmented Control component. It creates a control with three items: Inbox, Drafts, and Sent, with 'inbox' as the default selected value.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/segmented-control.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<SegmentedControl.Root defaultValue="inbox">
	<SegmentedControl.Item value="inbox">Inbox</SegmentedControl.Item>
	<SegmentedControl.Item value="drafts">Drafts</SegmentedControl.Item>
	<SegmentedControl.Item value="sent">Sent</SegmentedControl.Item>
</SegmentedControl.Root>
```

----------------------------------------

TITLE: Implementing Ghost Variant for Select Trigger in React
DESCRIPTION: This example shows how to use the 'ghost' trigger variant to render the Select trigger without a visually containing element. It compares the ghost variant with a surface variant.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/select.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Flex gap="3" align="center">
	<Select.Root defaultValue="apple">
		<Select.Trigger variant="surface" />
		<Select.Content>
			<Select.Item value="apple">Apple</Select.Item>
			<Select.Item value="orange">Orange</Select.Item>
		</Select.Content>
	</Select.Root>

	<Select.Root defaultValue="apple">
		<Select.Trigger variant="ghost" />
		<Select.Content>
			<Select.Item value="apple">Apple</Select.Item>
			<Select.Item value="orange">Orange</Select.Item>
		</Select.Content>
	</Select.Root>
</Flex>
```

----------------------------------------

TITLE: Setting Radio Group Variants in JSX
DESCRIPTION: Illustrates the use of the 'variant' prop to control the visual style of radio buttons with surface, classic, and soft options.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/radio-group.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Flex gap="2">
	<Flex direction="column" asChild gap="2">
		<RadioGroup.Root variant="surface" defaultValue="1">
			<RadioGroup.Item value="1" />
			<RadioGroup.Item value="2" />
		</RadioGroup.Root>
	</Flex>

	<Flex direction="column" asChild gap="2">
		<RadioGroup.Root variant="classic" defaultValue="1">
			<RadioGroup.Item value="1" />
			<RadioGroup.Item value="2" />
		</RadioGroup.Root>
	</Flex>

	<Flex direction="column" asChild gap="2">
		<RadioGroup.Root variant="soft" defaultValue="1">
			<RadioGroup.Item value="1" />
			<RadioGroup.Item value="2" />
		</RadioGroup.Root>
	</Flex>
</Flex>
```

----------------------------------------

TITLE: Implementing Checkbox Items in ContextMenu with React
DESCRIPTION: Example demonstrating how to add checkbox items to a ContextMenu with state management for checked status and visual indicators.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/context-menu.mdx#2025-04-21_snippet_15

LANGUAGE: jsx
CODE:
```
import * as React from "react";
import { CheckIcon } from "@radix-ui/react-icons";
import { ContextMenu } from "radix-ui";

export default () => {
	const [checked, setChecked] = React.useState(true);

	return (
		<ContextMenu.Root>
			<ContextMenu.Trigger>…</ContextMenu.Trigger>
			<ContextMenu.Portal>
				<ContextMenu.Content>
					<ContextMenu.Item>…</ContextMenu.Item>
					<ContextMenu.Item>…</ContextMenu.Item>
					<ContextMenu.Separator />
					<ContextMenu.CheckboxItem
						checked={checked}
						onCheckedChange={setChecked}
					>
						<ContextMenu.ItemIndicator>
							<CheckIcon />
						</ContextMenu.ItemIndicator>
						Checkbox item
					</ContextMenu.CheckboxItem>
				</ContextMenu.Content>
			</ContextMenu.Portal>
		</ContextMenu.Root>
	);
};
```

----------------------------------------

TITLE: Implementing checkbox items in Menubar (Radix UI, React)
DESCRIPTION: This example demonstrates how to create a checkbox item in a Radix UI Menubar using `Menubar.CheckboxItem`.  The `checked` state is managed using `React.useState`, and the `onCheckedChange` handler updates the state when the checkbox is toggled. The `Menubar.ItemIndicator` displays a check icon when the item is checked.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/menubar.mdx#2025-04-21_snippet_10

LANGUAGE: jsx
CODE:
```
import * as React from "react";
import { CheckIcon } from "@radix-ui/react-icons";
import { Menubar } from "radix-ui";

export default () => {
	const [checked, setChecked] = React.useState(true);

	return (
		<Menubar.Root>
			<Menubar.Menu>
				<Menubar.Trigger>…</Menubar.Trigger>
				<Menubar.Portal>
					<Menubar.Content>
						<Menubar.Item>…</Menubar.Item>
						<Menubar.Item>…</Menubar.Item>
						<Menubar.Separator />
						<Menubar.CheckboxItem
							checked={checked}
							onCheckedChange={setChecked}
						>
							<Menubar.ItemIndicator>
								<CheckIcon />
							</Menubar.ItemIndicator>
							Checkbox item
						</Menubar.CheckboxItem>
					</Menubar.Content>
				</Menubar.Portal>
			</Menubar.Menu>
		</Menubar.Root>
	);
};
```

----------------------------------------

TITLE: Allowing Collapsing of All Accordion Items
DESCRIPTION: Example of using the collapsible prop to allow all items to be closed.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/accordion.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Accordion.Root type="single" __collapsible__>
	<Accordion.Item value="item-1">…</Accordion.Item>
	<Accordion.Item value="item-2">…</Accordion.Item>
</Accordion.Root>
```

----------------------------------------

TITLE: Importing and Using Accessible Icon Component - JSX
DESCRIPTION: This example demonstrates how to import and use the Accessible Icon component from Radix UI in a React application. It exemplifies the structure and basic usage of the component with 'AccessibleIcon.Root'.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/utilities/accessible-icon.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { AccessibleIcon } from "radix-ui";

export default () => <AccessibleIcon.Root />;
```

----------------------------------------

TITLE: Component-level Theme Overrides in React
DESCRIPTION: This example demonstrates how to override theme configuration for specific components by passing supported props directly to those components. It shows custom styling for Switch and Button components.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/theme.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Box maxWidth="400px">
	<Card size="2">
		<Flex direction="column" gap="3">
			<Grid gap="1">
				<Text as="div" weight="bold" size="2" mb="1">
					Feedback
				</Text>
				<TextArea placeholder="Write your feedback…" />
			</Grid>
			<Flex asChild justify="between">
				<label>
					<Text color="gray" size="2">
						Attach screenshot?
					</Text>
					<Switch
						size="1"
						__color__="orange"
						__radius__="full"
						defaultChecked
					/>
				</label>
			</Flex>
			<Grid columns="2" gap="2">
				<Button variant="surface">Back</Button>
				<Button __color__="cyan" __radius__="full">
					Send
				</Button>
			</Grid>
		</Flex>
	</Card>
</Box>
```

----------------------------------------

TITLE: Color Override Example in React
DESCRIPTION: Shows how to override theme colors for nested components using the color prop
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/color.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
<Theme accentColor="indigo">
	<Flex align="start" direction={{ initial: "column", sm: "row" }} gap="4">
		<Callout.Root>
			<Callout.Icon>
				<InfoCircledIcon />
			</Callout.Icon>
			<Callout.Text>
				<Flex as="span" align="center" gap="4">
					<Text>There are new commits.</Text>
					<Button variant="surface" size="1" my="-2">
						Refresh
					</Button>
				</Flex>
			</Callout.Text>
		</Callout.Root>

		<Callout.Root color="gray">
			<Callout.Icon>
				<InfoCircledIcon />
			</Callout.Icon>
			<Callout.Text>
				<Flex as="span" align="center" gap="4">
					<Text>There are new commits.</Text>
					<Button variant="surface" size="1" my="-2">
						Refresh
					</Button>
				</Flex>
			</Callout.Text>
		</Callout.Root>
	</Flex>
</Theme>
```

----------------------------------------

TITLE: Implementing Radix UI Scroll Area Component in React
DESCRIPTION: Example of how to import and use the Scroll Area component parts in a React application.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/scroll-area.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { ScrollArea } from "radix-ui";

export default () => (
	<ScrollArea.Root>
		<ScrollArea.Viewport />
		<ScrollArea.Scrollbar orientation="horizontal">
			<ScrollArea.Thumb />
		</ScrollArea.Scrollbar>
		<ScrollArea.Scrollbar orientation="vertical">
			<ScrollArea.Thumb />
		</ScrollArea.Scrollbar>
		<ScrollArea.Corner />
	</ScrollArea.Root>
);
```

----------------------------------------

TITLE: Implementing Ghost Variant Icon Buttons with Multiple Colors in JSX
DESCRIPTION: This code shows how to create ghost variant icon buttons using gray, blue, and red colors from the Radix color scale. It demonstrates using transparent backgrounds in the default state and step 3 for hover states.
SOURCE: https://github.com/radix-ui/website/blob/main/data/colors/docs/palette-composition/understanding-the-scale.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Flex wrap="wrap" gap="5" my="5">
	<IconButton variant="ghost" color="gray">
		<PlusIcon />
	</IconButton>
	<IconButton variant="ghost" color="blue">
		<PlusIcon />
	</IconButton>
	<IconButton variant="ghost" color="red">
		<PlusIcon />
	</IconButton>
</Flex>
```

----------------------------------------

TITLE: Tabs Color Variants Example
DESCRIPTION: Demonstrates different color variations for tab lists including indigo, cyan, orange, and crimson options using the color prop.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/tabs.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="4" pb="2">
	<Tabs.Root defaultValue="account">
		<Tabs.List color="indigo">
			<Tabs.Trigger value="account">Account</Tabs.Trigger>
			<Tabs.Trigger value="documents">Documents</Tabs.Trigger>
			<Tabs.Trigger value="settings">Settings</Tabs.Trigger>
		</Tabs.List>
	</Tabs.Root>

	<Tabs.Root defaultValue="account">
		<Tabs.List color="cyan">
			<Tabs.Trigger value="account">Account</Tabs.Trigger>
			<Tabs.Trigger value="documents">Documents</Tabs.Trigger>
			<Tabs.Trigger value="settings">Settings</Tabs.Trigger>
		</Tabs.List>
	</Tabs.Root>

	<Tabs.Root defaultValue="account">
		<Tabs.List color="orange">
			<Tabs.Trigger value="account">Account</Tabs.Trigger>
			<Tabs.Trigger value="documents">Documents</Tabs.Trigger>
			<Tabs.Trigger value="settings">Settings</Tabs.Trigger>
		</Tabs.List>
	</Tabs.Root>

	<Tabs.Root defaultValue="account">
		<Tabs.List color="crimson">
			<Tabs.Trigger value="account">Account</Tabs.Trigger>
			<Tabs.Trigger value="documents">Documents</Tabs.Trigger>
			<Tabs.Trigger value="settings">Settings</Tabs.Trigger>
		</Tabs.List>
	</Tabs.Root>
</Flex>
```

----------------------------------------

TITLE: Setting Width on Box Component in JSX
DESCRIPTION: Shows examples of setting width on the Box component using fixed values and responsive object notation.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/overview/layout.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Box width="100px" />
<Box width={{ md: '100vw', xl: '1400px' }} />
```

----------------------------------------

TITLE: Disabled Checkbox States in JSX
DESCRIPTION: Shows how to implement disabled states for Checkbox components using the native disabled attribute. It adds checkboxes in different states (checked and unchecked) and demonstrates how a disabled style appears with some text wrapping using Flex and Text components.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/checkbox.mdx#2025-04-21_snippet_6

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="2">
	<Text as="label" size="2">
		<Flex as="span" gap="2">
			<Checkbox />
			Not checked
		</Flex>
	</Text>

	<Text as="label" size="2">
		<Flex as="span" gap="2">
			<Checkbox defaultChecked />
			Checked
		</Flex>
	</Text>

	<Text as="label" size="2" color="gray">
		<Flex as="span" gap="2">
			<Checkbox disabled />
			Not checked
		</Flex>
	</Text>

	<Text as="label" size="2" color="gray">
		<Flex as="span" gap="2">
			<Checkbox disabled defaultChecked />
			Checked
		</Flex>
	</Text>
</Flex>
```

----------------------------------------

TITLE: Custom Tooltip Implementation with Tooltip API in JavaScript
DESCRIPTION: This JavaScript snippet demonstrates the creation of a custom Tooltip component by abstracting Radix-UI's Tooltip parts. It uses the asChild prop to facilitate flexible trigger component integration. It requires React and Radix-UI as dependencies, with inputs consisting of children elements and optional props such as open and defaultOpen.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/tooltip.mdx#2025-04-21_snippet_8

LANGUAGE: jsx
CODE:
```
// your-tooltip.jsx
import * as React from "react";
import { Tooltip as TooltipPrimitive } from "radix-ui";

export function Tooltip({
	children,
	content,
	open,
	defaultOpen,
	onOpenChange,
	...props
}) {
	return (
		<TooltipPrimitive.Root
			open={open}
			defaultOpen={defaultOpen}
			onOpenChange={onOpenChange}
		>
			<TooltipPrimitive.Trigger __asChild__>
				{children}
			</TooltipPrimitive.Trigger>
			<TooltipPrimitive.Content side="top" align="center" {...props}>
				{content}
				<TooltipPrimitive.Arrow width={11} height={5} />
			</TooltipPrimitive.Content>
		</TooltipPrimitive.Root>
	);
}
```

----------------------------------------

TITLE: Creating Disabled Checkboxes - React
DESCRIPTION: This snippet showcases the creation of a Checkbox Group with items that can be disabled using the native `disabled` attribute. Disabled checkboxes visually prevent user interaction while being displayed.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/checkbox-group.mdx#2025-04-21_snippet_6

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="2">
	<CheckboxGroup.Root defaultValue="2">
		<CheckboxGroup.Item value="1">Off</CheckboxGroup.Item>
		<CheckboxGroup.Item value="2">On</CheckboxGroup.Item>
	</CheckboxGroup.Root>

	<CheckboxGroup.Root defaultValue="2">
		<CheckboxGroup.Item value="1" disabled>
			Off
		</CheckboxGroup.Item>
		<CheckboxGroup.Item value="2" disabled>
			On
		</CheckboxGroup.Item>
	</CheckboxGroup.Root>
</Flex>
```

----------------------------------------

TITLE: Adjust Orientation of DataList using Radix UI in JSX
DESCRIPTION: Utilizes the 'orientation' prop of the DataList component to control layout orientation. This snippet modifies the layout to be vertical initially or horizontal on small screens, showcasing the flexibility provided by Radix UI's responsive design capabilities. Requires the Radix UI component library.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/data-list.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<DataList.Root orientation={{ initial: "vertical", sm: "horizontal" }}>
	<DataList.Item>
		<DataList.Label minWidth="88px">Name</DataList.Label>
		<DataList.Value>Vlad Moroz</DataList.Value>
	</DataList.Item>
	<DataList.Item>
		<DataList.Label minWidth="88px">Email</DataList.Label>
		<DataList.Value>
			<Link href="mailto:vlad@workos.com">vlad@workos.com</Link>
		</DataList.Value>
	</DataList.Item>
	<DataList.Item>
		<DataList.Label minWidth="88px">Company</DataList.Label>
		<DataList.Value>
			<Link target="_blank" href="https://workos.com">
				WorkOS
			</Link>
		</DataList.Value>
	</DataList.Item>
</DataList.Root>
```

----------------------------------------

TITLE: Popover Custom API Usage (JSX)
DESCRIPTION: This example demonstrates how to create a custom API for the Popover component by abstracting the primitive parts into a new component. It uses the custom `Popover`, `PopoverTrigger`, and `PopoverContent` components.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/popover.mdx#2025-04-21_snippet_10

LANGUAGE: jsx
CODE:
```
"import { Popover, PopoverTrigger, PopoverContent } from \"./your-popover\";

export default () => (
	<Popover>
		<PopoverTrigger>Popover trigger</PopoverTrigger>
		<PopoverContent>Popover content</PopoverContent>
	</Popover>
);"
```

----------------------------------------

TITLE: Implementing Skeleton Component in React
DESCRIPTION: Demonstrates the usage of the new Skeleton component, which adopts the shape and size of child components to create loading placeholders that match the final layout.
SOURCE: https://github.com/radix-ui/website/blob/main/data/blog/themes-3.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Skeleton>
	<Button radius="full">Button</Button>
</Skeleton>
```

----------------------------------------

TITLE: Creating a Vertical Navigation Menu with Radix UI
DESCRIPTION: This code snippet demonstrates how to create a vertical navigation menu using the `orientation` prop on the `NavigationMenu.Root` component. It shows the basic structure with `NavigationMenu.List`, `NavigationMenu.Item`, `NavigationMenu.Trigger`, and `NavigationMenu.Content` components.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/navigation-menu.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<NavigationMenu.Root __orientation__="vertical">
	<NavigationMenu.List>
		<NavigationMenu.Item>
			<NavigationMenu.Trigger>Item one</NavigationMenu.Trigger>
			<NavigationMenu.Content>Item one content</NavigationMenu.Content>
		</NavigationMenu.Item>
		<NavigationMenu.Item>
			<NavigationMenu.Trigger>Item two</NavigationMenu.Trigger>
			<NavigationMenu.Content>Item Two content</NavigationMenu.Content>
		</NavigationMenu.Item>
	</NavigationMenu.List>
</NavigationMenu.Root>
```

----------------------------------------

TITLE: Applying Grid Children Props to Box Component in JSX
DESCRIPTION: Illustrates the use of grid children props on the Box component to control its placement and sizing within a grid container.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/overview/layout.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
<Box gridArea="header" />

<Box gridColumn="1 / 3" />
<Box gridColumnStart="2">
<Box gridColumnEnd={{ initial: "-1", md: "3", lg: "auto" }} />

<Box gridRow="1 / 3" />
<Box gridRowStart="2">
<Box gridRowEnd={{ initial: "-1", md: "3", lg: "auto" }} />
```

----------------------------------------

TITLE: Controlling Heading Size
DESCRIPTION: This code illustrates the size prop of the Heading component, which adjusts the heading size while maintaining appropriate line height and letter spacing.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/heading.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="3">
	<Heading size="1">The quick brown fox jumps over the lazy dog</Heading>
	<Heading size="2">The quick brown fox jumps over the lazy dog</Heading>
	<Heading size="3">The quick brown fox jumps over the lazy dog</Heading>
	<Heading size="4">The quick brown fox jumps over the lazy dog</Heading>
	<Heading size="5">The quick brown fox jumps over the lazy dog</Heading>
	<Heading size="6">The quick brown fox jumps over the lazy dog</Heading>
	<Heading size="7">The quick brown fox jumps over the lazy dog</Heading>
	<Heading size="8">The quick brown fox jumps over the lazy dog</Heading>
	<Heading size="9">The quick brown fox jumps over the lazy dog</Heading>
</Flex>
```

----------------------------------------

TITLE: Animating Swipe Gesture for Toast in React
DESCRIPTION: This snippet demonstrates how to animate a swipe-to-close gesture for the Toast component using CSS variables and data attributes. The __swipeDirection__ prop on the Toast.Provider enables the swipe gesture, and CSS styles are applied based on the data-swipe attribute to create the animation effect.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/toast.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
// index.jsx
import { Toast } from "radix-ui";
import "./styles.css";

export default () => (
	<Toast.Provider __swipeDirection__="right">
		<Toast.Root __className__="ToastRoot">...</Toast.Root>
		<Toast.Viewport />
	</Toast.Provider>
);
```

----------------------------------------

TITLE: Checkbox Color Assignment in JSX
DESCRIPTION: Illustrates the use of the color prop to assign different colors to Checkbox components within a Flex layout. Examples include colors like indigo, cyan, orange, and crimson, with checkboxes set to default checked.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/checkbox.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Flex gap="2">
	<Checkbox color="indigo" defaultChecked />
	<Checkbox color="cyan" defaultChecked />
	<Checkbox color="orange" defaultChecked />
	<Checkbox color="crimson" defaultChecked />
</Flex>
```

----------------------------------------

TITLE: Avatar Size Customization in a Flex Container
DESCRIPTION: This snippet illustrates how to use the 'size' prop to specify different sizes for Avatar components in a Flex container. Multiple avatars are displayed, each with varying sizes while maintaining a consistent source image and fallback character.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/avatar.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Flex align="center" gap="4">
	<Avatar
		size="1"
		src="https://images.unsplash.com/photo-1502823403499-6ccfcf4fb453?&w=256&h=256&q=70&crop=focalpoint&fp-x=0.5&fp-y=0.3&fp-z=1&fit=crop"
		fallback="A"
	/>
	<Avatar
		size="2"
		src="https://images.unsplash.com/photo-1502823403499-6ccfcf4fb453?&w=256&h=256&q=70&crop=focalpoint&fp-x=0.5&fp-y=0.3&fp-z=1&fit=crop"
		fallback="A"
	/>
	<Avatar
		size="3"
		src="https://images.unsplash.com/photo-1502823403499-6ccfcf4fb453?&w=256&h=256&q=70&crop=focalpoint&fp-x=0.5&fp-y=0.3&fp-z=1&fit=crop"
		fallback="A"
	/>
	<Avatar
		size="4"
		src="https://images.unsplash.com/photo-1502823403499-6ccfcf4fb453?&w=256&h=256&q=70&crop=focalpoint&fp-x=0.5&fp-y=0.3&fp-z=1&fit=crop"
		fallback="A"
	/>
	<Avatar
		size="5"
		src="https://images.unsplash.com/photo-1502823403499-6ccfcf4fb453?&w=256&h=256&q=70&crop=focalpoint&fp-x=0.5&fp-y=0.3&fp-z=1&fit=crop"
		fallback="A"
	/>
	<Avatar
		size="6"
		src="https://images.unsplash.com/photo-1502823403499-6ccfcf4fb453?&w=256&h=256&q=70&crop=focalpoint&fp-x=0.5&fp-y=0.3&fp-z=1&fit=crop"
		fallback="A"
	/>
	<Avatar
		size="7"
		src="https://images.unsplash.com/photo-1502823403499-6ccfcf4fb453?&w=256&h=256&q=70&crop=focalpoint&fp-x=0.5&fp-y=0.3&fp-z=1&fit=crop"
		fallback="A"
	/>
	<Avatar
		size="8"
		src="https://images.unsplash.com/photo-1502823403499-6ccfcf4fb453?&w=256&h=256&q=70&crop=focalpoint&fp-x=0.5&fp-y=0.3&fp-z=1&fit=crop"
		fallback="A"
	/>
</Flex>
```

----------------------------------------

TITLE: Implementing Basic Tooltip in React with Radix UI
DESCRIPTION: Example showing how to create a basic tooltip with an icon button. The tooltip displays 'Add to library' text when hovering over a button with a plus icon.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/tooltip.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Tooltip content="Add to library">
	<IconButton radius="full">
		<PlusIcon />
	</IconButton>
</Tooltip>
```

----------------------------------------

TITLE: Animating Content Size in Accordion with CSS
DESCRIPTION: This snippet uses CSS variables to animate the size of the accordion content when it opens or closes. The animations are defined using keyframes, and it is important to set the CSS variables for smooth and responsive animations. The expected functionality enhances the visual experience by providing smooth transitions.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/accordion.mdx#2025-04-21_snippet_8

LANGUAGE: jsx
CODE:
```
// index.jsx
import { Accordion } from "radix-ui";
import "./styles.css";

export default () => (
	<Accordion.Root type="single">
		<Accordion.Item value="item-1">
			<Accordion.Header>…</Accordion.Header>
			<Accordion.Content __className__="AccordionContent">…</Accordion.Content>
		</Accordion.Item>
	</Accordion.Root>
);
```

----------------------------------------

TITLE: Configuring Scroll Area Size in JSX
DESCRIPTION: This example shows how to use the 'size' prop to control the size of the scrollbar handles. It demonstrates three different sizes for horizontal scrollbars.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/scroll-area.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="2">
	<ScrollArea
		size="1"
		type="always"
		scrollbars="horizontal"
		style={{ width: 300, height: 12 }}
	>
		<Box width="800px" height="1px" />
	</ScrollArea>

	<ScrollArea
		size="2"
		type="always"
		scrollbars="horizontal"
		style={{ width: 350, height: 16 }}
	>
		<Box width="900px" height="1px" />
	</ScrollArea>

	<ScrollArea
		size="3"
		type="always"
		scrollbars="horizontal"
		style={{ width: 400, height: 20 }}
	>
		<Box width="1000px" height="1px" />
	</ScrollArea>
</Flex>
```

----------------------------------------

TITLE: Configuring Select Component Size in React
DESCRIPTION: This example shows how to use the 'size' prop to control the size of the Select component. It demonstrates three different sizes: 1, 2, and 3.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/select.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Flex gap="3" align="center">
	<Select.Root size="1" defaultValue="apple">
		<Select.Trigger />
		<Select.Content>
			<Select.Item value="apple">Apple</Select.Item>
			<Select.Item value="orange">Orange</Select.Item>
		</Select.Content>
	</Select.Root>

	<Select.Root size="2" defaultValue="apple">
		<Select.Trigger />
		<Select.Content>
			<Select.Item value="apple">Apple</Select.Item>
			<Select.Item value="orange">Orange</Select.Item>
		</Select.Content>
	</Select.Root>

	<Select.Root size="3" defaultValue="apple">
		<Select.Trigger />
		<Select.Content>
			<Select.Item value="apple">Apple</Select.Item>
			<Select.Item value="orange">Orange</Select.Item>
		</Select.Content>
	</Select.Root>
</Flex>
```

----------------------------------------

TITLE: Basic Slider Component Structure
DESCRIPTION: Shows the basic anatomy of the Slider component, importing and assembling all required parts including Root, Track, Range, and Thumb.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/slider.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { Slider } from "radix-ui";

export default () => (
	<Slider.Root>
		<Slider.Track>
			<Slider.Range />
		</Slider.Track>
		<Slider.Thumb />
	</Slider.Root>
);
```

----------------------------------------

TITLE: Styling disabled Menubar items (Radix UI, React)
DESCRIPTION: This example shows how to style disabled menu items using the `data-disabled` attribute. The JavaScript code creates a basic menu structure with a disabled item, while the CSS styles the disabled item with a specific color.  It relies on the `Menubar` component from `radix-ui` and a custom CSS class.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/menubar.mdx#2025-04-21_snippet_6

LANGUAGE: jsx
CODE:
```
// index.jsx
import { Menubar } from "radix-ui";
import "./styles.css";

export default () => (
	<Menubar.Root>
		<Menubar.Menu>
			<Menubar.Trigger>…</Menubar.Trigger>
			<Menubar.Portal>
				<Menubar.Content>
					<Menubar.Item __className__="MenubarItem" __disabled__>
						…
					</Menubar.Item>
					<Menubar.Item className="MenubarItem">…</Menubar.Item>
				</Menubar.Content>
			</Menubar.Portal>
		</Menubar.Menu>
	</Menubar.Root>
);
```

----------------------------------------

TITLE: Ghost Variant Icon Button in JSX
DESCRIPTION: This snippet demonstrates the `ghost` variant of the `IconButton`, which creates a button without any background or border. It displays only the icon. This example uses the MagnifyingGlassIcon.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/icon-button.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
"<IconButton variant=\"ghost\">\n\t<MagnifyingGlassIcon width=\"18\" height=\"18\" />\n</IconButton>"
```

----------------------------------------

TITLE: Defining Select Content Component Props
DESCRIPTION: Comprehensive props configuration for Select Content with positioning, event handling, and collision management options
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/select.mdx#2025-04-21_snippet_2

LANGUAGE: typescript
CODE:
```
{
  asChild?: boolean,
  onCloseAutoFocus?: (event: Event) => void,
  onEscapeKeyDown?: (event: KeyboardEvent) => void,
  position?: "item-aligned" | "popper",
  side?: "top" | "right" | "bottom" | "left",
  sideOffset?: number,
  align?: "start" | "center" | "end"
}
```

----------------------------------------

TITLE: Nested Theme Configuration in React
DESCRIPTION: This example shows how to nest Theme components to modify configuration for specific subtrees. It demonstrates inheritance of configuration from parent themes and overriding for child and grandchild components.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/theme.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Card size="2">
	<Flex gap="6">
		<Flex direction="column" gap="3">
			<Heading as="h5" size="2">
				Global
			</Heading>
			<Grid gap="1">
				<Text as="div" weight="bold" size="2" mb="1">
					Feedback
				</Text>
				<TextArea placeholder="Write your feedback…" />
			</Grid>
			<Button>Send</Button>
		</Flex>

		<Theme __accentColor__="cyan" __radius__="full">
			<Card size="2">
				<Flex gap="6">
					<Flex direction="column" gap="3">
						<Heading as="h5" size="2">
							Child
						</Heading>
						<Grid gap="1">
							<Text as="div" weight="bold" size="2" mb="1">
								Feedback
							</Text>
							<TextArea placeholder="Write your feedback…" />
						</Grid>
						<Button>Send</Button>
					</Flex>

					<Theme __accentColor__="orange">
						<Card size="2">
							<Flex direction="column" gap="3">
								<Heading as="h5" size="2">
									Grandchild
								</Heading>
								<Grid gap="1">
									<Text as="div" weight="bold" size="2" mb="1">
										Feedback
									</Text>
									<TextArea placeholder="Write your feedback…" />
								</Grid>
								<Button>Send</Button>
							</Flex>
						</Card>
					</Theme>
				</Flex>
			</Card>
		</Theme>
	</Flex>
</Card>
```

----------------------------------------

TITLE: Customizing Font Family in CSS
DESCRIPTION: Demonstrates how to override the default font family tokens to use custom fonts in Radix Themes.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/typography.mdx#2025-04-21_snippet_4

LANGUAGE: css
CODE:
```
.radix-themes {
	--default-font-family:  /* Your custom default font */ --heading-font-family:
		/* Your custom font for <Heading> components */ --code-font-family:
		/* Your custom font for <Code> components */ --strong-font-family:
		/* Your custom font for <Strong> components */ --em-font-family:
		/* Your custom font for <Em> components */ --quote-font-family:
		/* Your custom font for <Quote> components */;
}
```

----------------------------------------

TITLE: Using Controlled Values with One-Time Password Fields
DESCRIPTION: This example covers managing a controlled value in a One-Time Password Field. It provides a form setup that verifies a password against a valid code, updating the state on value change, and submitting the data automatically when filled.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/one-time-password-field.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
function Verify({ validCode }) {
	const [value, setValue] = React.useState("");
	const PASSWORD_LENGTH = 6;
	function handleSubmit() {
		if (value === validCode) {
			redirect("/authenticated");
		} else {
			window.alert("Invalid code");
		}
	}
	return (
		<OneTimePasswordField.Root
			autoSubmit
			value={value}
			onAutoSubmit={handleSubmit}
			onValueChange={setValue}
		>
			{PASSWORD_LENGTH.map((_, i) => (
				<OneTimePasswordField.Input key={i} />
			))}
		</OneTimePasswordField.Root>
	);
}
```

----------------------------------------

TITLE: Assigning Colors to Badge Components in JSX
DESCRIPTION: This example shows how to apply different colors to Badge components using the 'color' prop within a Flex container. Radix UI themes enable a rich color palette selection which is demonstrated here.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/badge.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Flex gap="2">\n	<Badge color="indigo">New</Badge>\n	<Badge color="cyan">New</Badge>\n	<Badge color="orange">New</Badge>\n	<Badge color="crimson">New</Badge>\n</Flex>
```

----------------------------------------

TITLE: Choosing Avatar Variants in a Flex Container
DESCRIPTION: This snippet demonstrates how to use the 'variant' prop to select the visual style of the Avatar components. It showcases two different variants of the Avatar within a Flex container, both using a fallback character.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/avatar.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Flex gap="2">
	<Avatar variant="solid" fallback="A" />
	<Avatar variant="soft" fallback="A" />
</Flex>
```

----------------------------------------

TITLE: Switch Component with Different Colors in JSX
DESCRIPTION: Demonstrates the Switch component with different color values using the color prop, including indigo, cyan, orange, and crimson.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/switch.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Flex gap="2">
	<Switch color="indigo" defaultChecked />
	<Switch color="cyan" defaultChecked />
	<Switch color="orange" defaultChecked />
	<Switch color="crimson" defaultChecked />
</Flex>
```

----------------------------------------

TITLE: Importing layout essentials CSS
DESCRIPTION: This snippet shows how to import the layout essentials CSS from Radix Themes.  This is required if you only want to use the layout components from Radix Themes.  JavaScript tree-shaking must be enabled to remove unused styles.
SOURCE: https://github.com/radix-ui/website/blob/main/data/blog/themes-3.mdx#2025-04-21_snippet_7

LANGUAGE: jsx
CODE:
```
import "@radix-ui/themes/layout.css";
```

----------------------------------------

TITLE: Flex component with responsive width
DESCRIPTION: This snippet demonstrates how to use responsive object syntax to set different widths for the Flex component at different breakpoints (initial, small, medium). The width will adjust based on the screen size.
SOURCE: https://github.com/radix-ui/website/blob/main/data/blog/themes-3.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
<Flex width={{ initial: "100%", sm: "300px", md: "500px" }} />
```

----------------------------------------

TITLE: Increasing Contrast with High-contrast Prop - React
DESCRIPTION: This snippet shows how to apply the `highContrast` prop to various Checkbox Groups to enhance visibility against the background, using the same base color. It encourages the use of high contrast for better accessibility.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/checkbox-group.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
<Grid rows="2" gap="2" display="inline-grid" flow="column">
	<CheckboxGroup.Root color="indigo" defaultValue="1">
		<CheckboxGroup.Item value="1" />
	</CheckboxGroup.Root>

	<CheckboxGroup.Root color="indigo" defaultValue="1" highContrast>
		<CheckboxGroup.Item value="1" />
	</CheckboxGroup.Root>

	<CheckboxGroup.Root color="cyan" defaultValue="1">
		<CheckboxGroup.Item value="1" />
	</CheckboxGroup.Root>

	<CheckboxGroup.Root color="cyan" defaultValue="1" highContrast>
		<CheckboxGroup.Item value="1" />
	</CheckboxGroup.Root>

	<CheckboxGroup.Root color="orange" defaultValue="1">
		<CheckboxGroup.Item value="1" />
	</CheckboxGroup.Root>

	<CheckboxGroup.Root color="orange" defaultValue="1" highContrast>
		<CheckboxGroup.Item value="1" />
	</CheckboxGroup.Root>

	<CheckboxGroup.Root color="crimson" defaultValue="1">
		<CheckboxGroup.Item value="1" />
	</CheckboxGroup.Root>

	<CheckboxGroup.Root color="crimson" defaultValue="1" highContrast>
		<CheckboxGroup.Item value="1" />
	</CheckboxGroup.Root>

	<CheckboxGroup.Root color="gray" defaultValue="1">
		<CheckboxGroup.Item value="1" />
	</CheckboxGroup.Root>

	<CheckboxGroup.Root color="gray" defaultValue="1" highContrast>
		<CheckboxGroup.Item value="1" />
	</CheckboxGroup.Root>
</Grid>
```

----------------------------------------

TITLE: Using the Portal Root Component to Render Content
DESCRIPTION: This snippet illustrates how to utilize the Portal component to render arbitrary content within the Portal's root. The content gets rendered in a separate DOM element within the document body by default.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/utilities/portal.mdx#2025-04-21_snippet_2

LANGUAGE: javascript
CODE:
```
import { Portal } from "radix-ui";

export default () => <Portal.Root>Content</Portal.Root>;
```

----------------------------------------

TITLE: Context Menu Size Variations
DESCRIPTION: Demonstrates how to control the context menu size using the `size` prop. Shows two different size configurations with identical menu structure.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/context-menu.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Grid columns="2" gap="3">\n\t<ContextMenu.Root>\n\t\t<ContextMenu.Trigger>\n\t\t\t<RightClickZone title="Size one" />\n\t\t</ContextMenu.Trigger>\n\t\t<ContextMenu.Content size="1">\n\t\t\t<ContextMenu.Item shortcut="⌘ E">Edit</ContextMenu.Item>\n\t\t\t<ContextMenu.Item shortcut="⌘ D">Duplicate</ContextMenu.Item>\n\t\t\t<ContextMenu.Separator />\n\t\t\t<ContextMenu.Item shortcut="⌘ N">Archive</ContextMenu.Item>\n\n\t\t\t<ContextMenu.Separator />\n\t\t\t<ContextMenu.Item shortcut="⌘ ⌫" color="red">\n\t\t\t\tDelete\n\t\t\t</ContextMenu.Item>\n\t\t</ContextMenu.Content>\n\t</ContextMenu.Root>\n\n\t<ContextMenu.Root>\n\t\t<ContextMenu.Trigger>\n\t\t\t<RightClickZone title="Size two" />\n\t\t</ContextMenu.Trigger>\n\t\t<ContextMenu.Content size="2">\n\t\t\t<ContextMenu.Item shortcut="⌘ E">Edit</ContextMenu.Item>\n\t\t\t<ContextMenu.Item shortcut="⌘ D">Duplicate</ContextMenu.Item>\n\t\t\t<ContextMenu.Separator />\n\t\t\t<ContextMenu.Item shortcut="⌘ N">Archive</ContextMenu.Item>\n\n\t\t\t<ContextMenu.Separator />\n\t\t\t<ContextMenu.Item shortcut="⌘ ⌫" color="red">\n\t\t\t\tDelete\n\t\t\t</ContextMenu.Item>\n\t\t</ContextMenu.Content>\n\t</ContextMenu.Root>\n</Grid>
```

----------------------------------------

TITLE: Vertical Tabs Implementation
DESCRIPTION: Example showing how to implement vertical tabs using the orientation prop, including triggers and content panels.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/tabs.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
import { Tabs } from "radix-ui";

export default () => (
	<Tabs.Root defaultValue="tab1" __orientation__="vertical">
		<Tabs.List aria-label="tabs example">
			<Tabs.Trigger value="tab1">One</Tabs.Trigger>
			<Tabs.Trigger value="tab2">Two</Tabs.Trigger>
			<Tabs.Trigger value="tab3">Three</Tabs.Trigger>
		</Tabs.List>
		<Tabs.Content value="tab1">Tab one content</Tabs.Content>
		<Tabs.Content value="tab2">Tab two content</Tabs.Content>
		<Tabs.Content value="tab3">Tab three content</Tabs.Content>
	</Tabs.Root>
);
```

----------------------------------------

TITLE: Creating a Range Slider
DESCRIPTION: Shows how to implement a range slider with multiple thumbs by providing an array of values and adding multiple Thumb components.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/slider.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
import { Slider } from "radix-ui";

export default () => (
	<Slider.Root defaultValue={__[25, 75]__}>
		<Slider.Track>
			<Slider.Range />
		</Slider.Track>
		<Slider.Thumb />
		<Slider.Thumb />
	</Slider.Root>
);
```

----------------------------------------

TITLE: Enabling Multiple Open Items in Accordion
DESCRIPTION: Example of setting the type prop to 'multiple' to allow multiple items to be open simultaneously.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/accordion.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
<Accordion.Root type="__multiple__">
	<Accordion.Item value="item-1">…</Accordion.Item>
	<Accordion.Item value="item-2">…</Accordion.Item>
</Accordion.Root>
```

----------------------------------------

TITLE: Styling Radix UI Accordion Item state with CSS
DESCRIPTION: This code snippet shows how to style the state of a Radix UI Accordion Item using the `data-state` attribute in CSS. When the accordion item is open, it includes `data-state="open"`, which is targeted in CSS to apply different styles.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/guides/styling.mdx#2025-04-21_snippet_1

LANGUAGE: css
CODE:
```
.AccordionItem {
	border-bottom: 1px solid gainsboro;
}

.AccordionItem[__data-state__="open"] {
	border-bottom-width: 2px;
}
```

----------------------------------------

TITLE: Text Field Color Variants
DESCRIPTION: Examples of text fields with different color assignments
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/text-field.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="3" maxWidth="250px">
	<TextField.Root
		color="indigo"
		variant="soft"
		placeholder="Search the docs…"
	/>
	<TextField.Root color="green" variant="soft" placeholder="Search the docs…" />
	<TextField.Root color="red" variant="soft" placeholder="Search the docs…" />
</Flex>
```

----------------------------------------

TITLE: Text Component with High Contrast in JSX
DESCRIPTION: Example demonstrating the 'highContrast' prop to increase color contrast with the background, making text more readable especially for accessibility concerns.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/text.mdx#2025-04-21_snippet_15

LANGUAGE: jsx
CODE:
```
<Flex direction="column">
	<Text color="gray">The quick brown fox jumps over the lazy dog.</Text>
	<Text color="gray" highContrast>
		The quick brown fox jumps over the lazy dog.
	</Text>
</Flex>
```

----------------------------------------

TITLE: Tabs High Contrast Example
DESCRIPTION: Shows how to implement high contrast tabs using the highContrast prop, comparing regular and high contrast variations with gray color.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/tabs.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="4" pb="2">
	<Tabs.Root defaultValue="account">
		<Tabs.List color="gray">
			<Tabs.Trigger value="account">Account</Tabs.Trigger>
			<Tabs.Trigger value="documents">Documents</Tabs.Trigger>
			<Tabs.Trigger value="settings">Settings</Tabs.Trigger>
		</Tabs.List>
	</Tabs.Root>

	<Tabs.Root defaultValue="account">
		<Tabs.List color="gray" highContrast>
			<Tabs.Trigger value="account">Account</Tabs.Trigger>
			<Tabs.Trigger value="documents">Documents</Tabs.Trigger>
			<Tabs.Trigger value="settings">Settings</Tabs.Trigger>
		</Tabs.List>
	</Tabs.Root>
</Flex>
```

----------------------------------------

TITLE: Using High-contrast Prop in Blockquote Component with JSX
DESCRIPTION: This snippet illustrates the usage of the 'highContrast' prop to increase the color contrast of the Blockquote against its background, demonstrated alongside a regular colored Blockquote in a Flex layout.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/blockquote.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="3" maxWidth="500px">
	<Blockquote color="gray">
		Perfect typography is certainly the most elusive of all arts. Sculpture in
		stone alone comes near it in obstinacy.
	</Blockquote>
	<Blockquote color="gray" highContrast>
		Perfect typography is certainly the most elusive of all arts. Sculpture in
		stone alone comes near it in obstinacy.
	</Blockquote>
</Flex>
```

----------------------------------------

TITLE: Using PasswordToggleField with Slot and Render Prop (JSX)
DESCRIPTION: Illustrates using the PasswordToggleField.Slot component with a 'render' prop. The render prop receives the current 'visible' state and returns a custom element, in this case, an SVG, allowing for dynamic rendering based on visibility.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/password-toggle-field.mdx#_snippet_3

LANGUAGE: jsx
CODE:
```
<PasswordToggleField.Root>
	<PasswordToggleField.Input />
	<PasswordToggleField.Toggle>
		<PasswordToggleField.Slot
			render={({ visible }) => (
				<svg aria-hidden viewBox="0 0 2 2" xmlns="http://www.w3.org/2000/svg">
					<path d={visible ? "M1 1 L2 2" : "M2 1 L1 2"} />
				</svg>
			)}
		/>
	</PasswordToggleField.Toggle>
</PasswordToggleField.Root>
```

----------------------------------------

TITLE: Accent Color CSS Variables
DESCRIPTION: Lists all available CSS variables for accent colors including backgrounds, interactive components, borders, and text colors
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/color.mdx#2025-04-21_snippet_1

LANGUAGE: css
CODE:
```
/* Backgrounds */
var(--accent-1);
var(--accent-2);

/* Interactive components */
var(--accent-3);
var(--accent-4);
var(--accent-5);

/* Borders and separators */
var(--accent-6);
var(--accent-7);
var(--accent-8);

/* Solid colors */
var(--accent-9);
var(--accent-10);

/* Accessible text */
var(--accent-11);
var(--accent-12);

/* Functional colors */
var(--accent-surface);
var(--accent-indicator);
var(--accent-track);
var(--accent-contrast);
```

----------------------------------------

TITLE: Basic Tab Navigation Implementation in JSX
DESCRIPTION: Basic implementation of a tab navigation menu with three links, where the first tab is active.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/tab-nav.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<TabNav.Root>
	<TabNav.Link href="#" active>
		Account
	</TabNav.Link>
	<TabNav.Link href="#">Documents</TabNav.Link>
	<TabNav.Link href="#">Settings</TabNav.Link>
</TabNav.Root>
```

----------------------------------------

TITLE: TextArea Variant Styles
DESCRIPTION: Shows different visual styles for the TextArea using the variant prop with surface, classic, and soft options.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/text-area.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="3" maxWidth="250px">
	<TextArea variant="surface" placeholder="Reply to comment…" />
	<TextArea variant="classic" placeholder="Reply to comment…" />
	<TextArea variant="soft" placeholder="Reply to comment…" />
</Flex>
```

----------------------------------------

TITLE: Styling Radix UI Accordion Item with styled-components
DESCRIPTION: This example shows how to style a Radix UI Accordion Item using styled-components. It imports the Accordion and styled-components libraries, then uses the `styled` function to create a styled Accordion.Item component.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/guides/styling.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
import * as React from "react";
import { Accordion } from "radix-ui";
import styled from "styled-components";

const StyledItem = __styled__(Accordion.Item)`
  border-bottom: 1px solid gainsboro;
`;

const AccordionDemo = () => (
	<Accordion.Root>
		<StyledItem value="item-1" />
		{/* … */}
	</Accordion.Root>
);

export default AccordionDemo;
```

----------------------------------------

TITLE: Long Text Skeleton Example
DESCRIPTION: Demonstrates the difference in skeleton behavior when wrapping longer paragraphs of text content.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/skeleton.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
<Container size="1">
	<Flex direction="column" gap="3">
		<Text>
			<Skeleton>
				Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque
				felis tellus, efficitur id convallis a, viverra eget libero. Nam magna
				erat, fringilla sed commodo sed, aliquet nec magna.
			</Skeleton>
		</Text>

		<Skeleton>
			<Text>
				Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque
				felis tellus, efficitur id convallis a, viverra eget libero. Nam magna
				erat, fringilla sed commodo sed, aliquet nec magna.
			</Text>
		</Skeleton>
	</Flex>
</Container>
```

----------------------------------------

TITLE: Rendering a Spinner Component in React
DESCRIPTION: Demonstrates how to use the new Spinner component in Radix Themes 3.0. The Spinner is a simple animated loading indicator that can conditionally render its children when loading is complete.
SOURCE: https://github.com/radix-ui/website/blob/main/data/blog/themes-3.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Spinner />
```

----------------------------------------

TITLE: Basic Card Implementation with Avatar and Text - JSX
DESCRIPTION: This snippet demonstrates a basic implementation of the Radix UI Card component, incorporating the Avatar and Text components for displaying user information. The Card is used to group the avatar and text elements, creating a visually cohesive unit. The `maxWidth` prop on the Box limits the card's width.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/card.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
"<Box maxWidth=\"240px\">\n\t<Card>\n\t	<Flex gap=\"3\" align=\"center\">\n\t		<Avatar\n\t			size=\"3\"\n\t			src=\"https://images.unsplash.com/photo-1607346256330-dee7af15f7c5?&w=64&h=64&dpr=2&q=70&crop=focalpoint&fp-x=0.67&fp-y=0.5&fp-z=1.4&fit=crop\"\n\t			radius=\"full\"\n\t			fallback=\"T\"\n\t		/>\n\t		<Box>\n\t			<Text as=\"div\" size=\"2\" weight=\"bold\">\n\t				Teodros Girmay\n\t			</Text>\n\t			<Text as=\"div\" size=\"2\" color=\"gray\">\n\t				Engineering\n\t			</Text>\n\t		</Box>\n\t	</Flex>\n\t</Card>\n</Box>"
```

----------------------------------------

TITLE: Implementing Radio Items in DropdownMenu with React
DESCRIPTION: This example shows how to add radio items to a DropdownMenu using the RadioGroup and RadioItem parts, managing their state with React hooks.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/dropdown-menu.mdx#2025-04-21_snippet_12

LANGUAGE: jsx
CODE:
```
import * as React from "react";
import { CheckIcon } from "@radix-ui/react-icons";
import { DropdownMenu } from "radix-ui";

export default () => {
	const [color, setColor] = React.useState("blue");

	return (
		<DropdownMenu.Root>
			<DropdownMenu.Trigger>…</DropdownMenu.Trigger>
			<DropdownMenu.Portal>
				<DropdownMenu.Content>
					<DropdownMenu.RadioGroup value={color} onValueChange={setColor}>
						<DropdownMenu.RadioItem value="red">
							<DropdownMenu.ItemIndicator>
								<CheckIcon />
							</DropdownMenu.ItemIndicator>
							Red
						</DropdownMenu.RadioItem>
						<DropdownMenu.RadioItem value="blue">
							<DropdownMenu.ItemIndicator>
								<CheckIcon />
							</DropdownMenu.ItemIndicator>
							Blue
						</DropdownMenu.RadioItem>
						<DropdownMenu.RadioItem value="green">
							<DropdownMenu.ItemIndicator>
								<CheckIcon />
							</DropdownMenu.ItemIndicator>
							Green
						</DropdownMenu.RadioItem>
					</DropdownMenu.RadioGroup>
				</DropdownMenu.Content>
			</DropdownMenu.Portal>
		</DropdownMenu.Root>
	);
};
```

----------------------------------------

TITLE: Popover with Collision-Aware Animations (JSX)
DESCRIPTION: This example demonstrates how to use the `data-side` and `data-align` attributes exposed by the Radix UI Popover component to create collision and direction-aware animations. It imports the Popover component and defines a basic Popover structure with a Trigger and Content.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/popover.mdx#2025-04-21_snippet_6

LANGUAGE: jsx
CODE:
```
"// index.jsx
import { Popover } from \"radix-ui\";
import \"./styles.css\";

export default () => (
	<Popover.Root>
		<Popover.Trigger>…</Popover.Trigger>
		<Popover.Portal>
			<Popover.Content __className__=\"PopoverContent\">…</Popover.Content>
		</Popover.Portal>
	</Popover.Root>
);"
```

----------------------------------------

TITLE: Creating Collision-aware Animations for DropdownMenu
DESCRIPTION: Demonstrates using data-side and data-align attributes exposed by Radix UI to create animations that adapt to collision detection and positioning changes at runtime.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/dropdown-menu.mdx#2025-04-21_snippet_16

LANGUAGE: jsx
CODE:
```
// index.jsx
import { DropdownMenu } from "radix-ui";
import "./styles.css";

export default () => (
	<DropdownMenu.Root>
		<DropdownMenu.Trigger>…</DropdownMenu.Trigger>
		<DropdownMenu.Portal>
			<DropdownMenu.Content __className__="DropdownMenuContent">
				…
			</DropdownMenu.Content>
		</DropdownMenu.Portal>
	</DropdownMenu.Root>
);
```

LANGUAGE: css
CODE:
```
/* styles.css */
.DropdownMenuContent {
	animation-duration: 0.6s;
	animation-timing-function: cubic-bezier(0.16, 1, 0.3, 1);
}
.DropdownMenuContent[__data-side="top"__] {
	animation-name: slideUp;
}
.DropdownMenuContent[__data-side="bottom"__] {
	animation-name: slideDown;
}

@keyframes slideUp {
	from {
		opacity: 0;
		transform: translateY(10px);
	}
	to {
		opacity: 1;
		transform: translateY(0);
	}
}

@keyframes slideDown {
	from {
		opacity: 0;
		transform: translateY(-10px);
	}
	to {
		opacity: 1;
		transform: translateY(0);
	}
}
```

----------------------------------------

TITLE: Controlling Badge Size in JSX
DESCRIPTION: This snippet showcases how to use the 'size' prop to modify the size of Badge components rendered within a Flex layout. Various sizes are applied to Badge elements to demonstrate customizable UI options. Requires the Radix UI library and Flex component.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/badge.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Flex align="center" gap="2">\n	<Badge size="1" color="indigo">\n		New\n	</Badge>\n	<Badge size="2" color="indigo">\n		New\n	</Badge>\n	<Badge size="3" color="indigo">\n		New\n	</Badge>\n</Flex>
```

----------------------------------------

TITLE: Styling Form Components Based on Validity
DESCRIPTION: Demonstrates how to apply CSS styles to form elements using data attributes for validity states
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/form.mdx#2025-04-21_snippet_6

LANGUAGE: jsx
CODE:
```
import * as React from "react";
import { Form } from "radix-ui";

export default () => (
	<Form.Root>
		<Form.Field name="email">
			<Form.Label __className__="FormLabel">Email</Form.Label>
			<Form.Control type="email" />
		</Form.Field>
	</Form.Root>
);
```

LANGUAGE: css
CODE:
```
/* styles.css */
.FormLabel[__data-invalid__] {
	color: red;
}
.FormLabel[__data-valid__] {
	color: green;
}
```

----------------------------------------

TITLE: Using Imperative Toast API in React
DESCRIPTION: This snippet demonstrates how to use the imperative Toast API created using React's forwardRef and useImperativeHandle hooks. It creates a ref to the Toast component and calls the publish method on the ref to trigger the toast display imperatively. It allows to duplicate toasts.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/toast.mdx#2025-04-21_snippet_13

LANGUAGE: jsx
CODE:
```
import { Toast } from "./your-toast";

export default () => {
	const savedRef = React.useRef();
	return (
		<div>
			<form onSubmit={() => savedRef.current.publish()}>
				{/* ... */}
				<button>Save</button>
			</form>
			<Toast ref={savedRef}>Saved successfully!</Toast>
		</div>
	);
};
```

----------------------------------------

TITLE: Displaying User List within a Dialog Using Radix UI
DESCRIPTION: This snippet shows how to create a dialog that displays a list of users. It utilizes Radix UI components to structure the content including a title and description, and incorporates an inset component for layout adjustments.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/dialog.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Dialog.Root>
	<Dialog.Trigger>
		<Button>View users</Button>
	</Dialog.Trigger>
	<Dialog.Content>
		<Dialog.Title>Users</Dialog.Title>
		<Dialog.Description>
			The following users have access to this project.
		</Dialog.Description>

		<Inset side="x" my="5">
			<Table.Root>
				<Table.Header>
					<Table.Row>
						<Table.ColumnHeaderCell>Full name</Table.ColumnHeaderCell>
						<Table.ColumnHeaderCell>Email</Table.ColumnHeaderCell>
						<Table.ColumnHeaderCell>Group</Table.ColumnHeaderCell>
					</Table.Row>
				</Table.Header>

				<Table.Body>
					<Table.Row>
						<Table.RowHeaderCell>Danilo Sousa</Table.RowHeaderCell>
						<Table.Cell>danilo@example.com</Table.Cell>
						<Table.Cell>Developer</Table.Cell>
					</Table.Row>

					<Table.Row>
						<Table.RowHeaderCell>Zahra Ambessa</Table.RowHeaderCell>
						<Table.Cell>zahra@example.com</Table.Cell>
						<Table.Cell>Admin</Table.Cell>
					</Table.Row>
				</Table.Body>
				</Table.Root>
			</Inset>

			<Flex gap="3" justify="end">
				<Dialog.Close>
					<Button variant="soft" color="gray">
						Close
					</Button>
				</Dialog.Close>
			</Flex>
		</Dialog.Content>
</Dialog.Root>
```

----------------------------------------

TITLE: Checkbox Visual Variants in JSX
DESCRIPTION: This snippet shows how to use the variant prop to apply different visual styles to Checkbox components. It presents three different styles: surface, classic, and soft, with variations of checked and unchecked states. The Flex component is used to structure these differently styled checkboxes.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/checkbox.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Flex align="center" gap="4">
	<Flex gap="2">
		<Checkbox variant="surface" defaultChecked />
		<Checkbox variant="surface" />
	</Flex>

	<Flex gap="2">
		<Checkbox variant="classic" defaultChecked />
		<Checkbox variant="classic" />
	</Flex>

	<Flex gap="2">
		<Checkbox variant="soft" defaultChecked />
		<Checkbox variant="soft" />
	</Flex>
</Flex>
```

----------------------------------------

TITLE: Creating Disabled Radio Buttons in JSX
DESCRIPTION: This snippet shows how to create disabled radio buttons using the native 'disabled' attribute. It displays both enabled and disabled radio buttons for comparison.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/radio.mdx#2025-04-21_snippet_6

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="3">
	<Flex align="start" direction="column" gap="1">
		<Flex asChild gap="2">
			<Text as="label" size="2">
				<Radio name="enabled" value="1" defaultChecked />
				On
			</Text>
		</Flex>
		<Flex asChild gap="2">
			<Text as="label" size="2">
				<Radio name="enabled" value="2" />
				Off
			</Text>
		</Flex>
	</Flex>

	<Flex align="start" direction="column" gap="1">
		<Flex asChild gap="2">
			<Text as="label" size="2" color="gray">
				<Radio disabled name="disabled" value="1" defaultChecked />
				On
			</Text>
		</Flex>
		<Flex asChild gap="2">
			<Text as="label" size="2" color="gray">
				<Radio disabled name="disabled" value="2" />
				Off
			</Text>
		</Flex>
	</Flex>
</Flex>
```

----------------------------------------

TITLE: Indeterminate Checkbox States in JSX
DESCRIPTION: Demonstrates creating an indeterminate Checkbox component using the defaultChecked and checked attributes with the "indeterminate" value. Flex is used to display checkboxes side by side.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/checkbox.mdx#2025-04-21_snippet_7

LANGUAGE: jsx
CODE:
```
<Flex gap="2">
	<Checkbox defaultChecked="indeterminate" />
	<Checkbox checked="indeterminate" />
</Flex>
```

----------------------------------------

TITLE: Example of Direction Provider with RTL Direction
DESCRIPTION: This code snippet illustrates how to use the Direction.Provider with a specified reading direction of 'rtl' (right-to-left). This example demonstrates how to set the global reading direction for all primitives in the application.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/utilities/direction-provider.mdx#2025-04-21_snippet_2

LANGUAGE: JavaScript
CODE:
```
import { Direction } from "radix-ui";

export default () => (
	<Direction.Provider dir="rtl">
		{/* your app */}
	</Direction.Provider>
);
```

----------------------------------------

TITLE: High Contrast Tab Navigation Implementation
DESCRIPTION: Example showing the usage of highContrast prop to increase color contrast with the background.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/tab-nav.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="4" pb="2">
	<TabNav.Root color="gray">
		<TabNav.Link href="#" active>
			Account
		</TabNav.Link>
		<TabNav.Link href="#">Documents</TabNav.Link>
		<TabNav.Link href="#">Settings</TabNav.Link>
	</TabNav.Root>

	<TabNav.Root color="gray" highContrast>
		<TabNav.Link href="#" active>
			Account
		</TabNav.Link>
		<TabNav.Link href="#">Documents</TabNav.Link>
		<TabNav.Link href="#">Settings</TabNav.Link>
	</TabNav.Root>
</Flex>
```

----------------------------------------

TITLE: Basic Form Component Structure in React
DESCRIPTION: Example of how to import and structure the Form component parts in a React application.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/form.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { Form } from "radix-ui";

export default () => (
	<Form.Root>
		<Form.Field>
			<Form.Label />
			<Form.Control />
			<Form.Message />
			<Form.ValidityState />
		</Form.Field>

		<Form.Message />
		<Form.ValidityState />

		<Form.Submit />
	</Form.Root>
);
```

----------------------------------------

TITLE: Initializing Checkbox Group Component - React
DESCRIPTION: This snippet initializes a Checkbox Group component with a default value and defines several Checkbox items within it. Each item is associated with a value representing its identification. It allows for multiple selections.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/checkbox-group.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<CheckboxGroup.Root defaultValue={["1"]} name="example">
	<CheckboxGroup.Item value="1">Fun</CheckboxGroup.Item>
	<CheckboxGroup.Item value="2">Serious</CheckboxGroup.Item>
	<CheckboxGroup.Item value="3">Smart</CheckboxGroup.Item>
</CheckboxGroup.Root>
```

----------------------------------------

TITLE: Enhancing Callout Contrast in React
DESCRIPTION: Shows how the `highContrast` prop can be employed to improve the contrast of a callout component. This feature enhances readability against various backgrounds, offering a visually distinct option for users who may need it. Kit requirements involve Radix UI components with contrast-themed styles.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/callout.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
<Flex direction=\"column\" gap=\"3\">\n\t<Callout.Root color=\"gray\" variant=\"soft\">\n\t\t<Callout.Icon>\n\t\t\t<InfoCircledIcon />\n\t\t</Callout.Icon>\n\t\t<Callout.Text>\n\t\t\tAn update to Radix Themes is available. See what’s new in version 3.2.0.\n\t\t</Callout.Text>\n\t</Callout.Root>\n\n\t<Callout.Root color=\"gray\" variant=\"soft\" highContrast>\n\t\t<Callout.Icon>\n\t\t\t<InfoCircledIcon />\n\t\t</Callout.Icon>\n\t\t<Callout.Text>\n\t\t\tAn update to Radix Themes is available. See what’s new in version 3.2.0.\n\t\t</Callout.Text>\n\t</Callout.Root>\n</Flex>
```

----------------------------------------

TITLE: Creating Submenus with Radix UI Navigation Menu
DESCRIPTION: This code demonstrates how to create a submenu using the `NavigationMenu.__Sub__` component within the main `NavigationMenu`. It shows how to nest navigation menus and assign a `defaultValue` to the submenu to control the active item.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/navigation-menu.mdx#2025-04-21_snippet_6

LANGUAGE: jsx
CODE:
```
<NavigationMenu.Root>
	<NavigationMenu.List>
		<NavigationMenu.Item>
			<NavigationMenu.Trigger>Item one</NavigationMenu.Trigger>
			<NavigationMenu.Content>Item one content</NavigationMenu.Content>
		</NavigationMenu.Item>
		<NavigationMenu.Item>
			<NavigationMenu.Trigger>Item two</NavigationMenu.Trigger>
			<NavigationMenu.Content>
				<NavigationMenu.__Sub__ __defaultValue__="sub1">
					<NavigationMenu.List>
						<NavigationMenu.Item value="sub1">
							<NavigationMenu.Trigger>Sub item one</NavigationMenu.Trigger>
							<NavigationMenu.Content>
								Sub item one content
							</NavigationMenu.Content>
						</NavigationMenu.Item>
						<NavigationMenu.Item value="sub2">
							<NavigationMenu.Trigger>Sub item two</NavigationMenu.Trigger>
							<NavigationMenu.Content>
								Sub item two content
							</NavigationMenu.Content>
						</NavigationMenu.Item>
					</NavigationMenu.List>
				</NavigationMenu.__Sub__>
			</NavigationMenu.Content>
		</NavigationMenu.Item>
	</NavigationMenu.List>
</NavigationMenu.Root>
```

----------------------------------------

TITLE: Creating submenus with Menubar.SubContent (Radix UI, React)
DESCRIPTION: This example demonstrates how to create a submenu using `Menubar.Sub`, `Menubar.SubTrigger`, and `Menubar.SubContent` components. The `Menubar.SubContent` contains menu items and an arrow, forming the submenu's content. It relies on other Menubar components for structure and functionality.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/menubar.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
<Menubar.Root>
	<Menubar.Menu>
		<Menubar.Trigger>…</Menubar.Trigger>
		<Menubar.Portal>
			<Menubar.Content>
				<Menubar.Item>…</Menubar.Item>
				<Menubar.Item>…</Menubar.Item>
				<Menubar.Separator />
				<Menubar.Sub>
					<Menubar.SubTrigger>Sub menu →</Menubar.SubTrigger>
					<Menubar.Portal>
						<Menubar.SubContent>
							<Menubar.Item>Sub menu item</Menubar.Item>
							<Menubar.Item>Sub menu item</Menubar.Item>
							<Menubar.Arrow />
						</Menubar.SubContent>
					</Menubar.Portal>
				</Menubar.Sub>
				<Menubar.Separator />
				<Menubar.Item>…</Menubar.Item>
			</Menubar.Content>
		</Menubar.Portal>
	</Menubar.Menu>
</Menubar.Root>
```

----------------------------------------

TITLE: Creating an Inset Content Popover with Radix UI in JSX
DESCRIPTION: This example shows how to create a Popover in Radix UI that incorporates inset alignment using JSX. It features an image and text aligned using a Grid layout, allowing for responsive design within the Popover.Content. The Radix UI framework's Inset component is used for precise positioning, and the snippet depends on Radix UI primitives to function properly.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/popover.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Popover.Root>
	<Popover.Trigger>
		<Button variant="soft">
			<Share2Icon width="16" height="16" />
			Share image
		</Button>
	</Popover.Trigger>
	<Popover.Content width="360px">
		<Grid columns="130px 1fr">
			<Inset side="left" pr="current">
				<img
					src="https://images.unsplash.com/photo-1618005182384-a83a8bd57fbe?&auto=format&fit=crop&w=400&q=80"
					style={{ objectFit: "cover", width: "100%", height: "100%" }}
				/>
			</Inset>

			<div>
				<Heading size="2" mb="1">
					Share this image
				</Heading>
				<Text as="p" size="2" mb="4" color="gray">
					Minimalistic 3D rendering wallpaper.
				</Text>

				<Flex direction="column" align="stretch">
					<Popover.Close>
						<Button size="1" variant="soft">
							<Link1Icon width="16" height="16" />
							Copy link
						</Button>
					</Popover.Close>
				</Flex>
			</div>
		</Grid>
	</Popover.Content>
</Popover.Root>
```

----------------------------------------

TITLE: Enhancing Dropdown Menu Accessibility with High-Contrast Prop
DESCRIPTION: This snippet illustrates how to apply the `highContrast` prop to enhance the dropdown menu's accessibility by increasing color contrast with the background. It provides various examples of toggle settings for triggering buttons and content items.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/dropdown-menu.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
<Grid columns="2" gap="3" display="inline-grid">
	<DropdownMenu.Root>
		<DropdownMenu.Trigger>
			<Button color="gray">
				Options
				<DropdownMenu.TriggerIcon />
			</Button>
		</DropdownMenu.Trigger>
		<DropdownMenu.Content color="gray">
			<DropdownMenu.Item shortcut="⌘ E">Edit</DropdownMenu.Item>
			<DropdownMenu.Item shortcut="⌘ D">Duplicate</DropdownMenu.Item>
			<DropdownMenu.Separator />
			<DropdownMenu.Item shortcut="⌘ N">Archive</DropdownMenu.Item>
		</DropdownMenu.Content>
	</DropdownMenu.Root>

	<DropdownMenu.Root>
		<DropdownMenu.Trigger>
			<Button color="gray" highContrast>
				Options
				<DropdownMenu.TriggerIcon />
			</Button>
		</DropdownMenu.Trigger>
		<DropdownMenu.Content color="gray" highContrast>
			<DropdownMenu.Item shortcut="⌘ E">Edit</DropdownMenu.Item>
			<DropdownMenu.Item shortcut="⌘ D">Duplicate</DropdownMenu.Item>
			<DropdownMenu.Separator />
			<DropdownMenu.Item shortcut="⌘ N">Archive</DropdownMenu.Item>
		</DropdownMenu.Content>
	</DropdownMenu.Root>

	<DropdownMenu.Root>
		<DropdownMenu.Trigger>
			<Button color="gray" variant="soft">
				Options
				<DropdownMenu.TriggerIcon />
			</Button>
		</DropdownMenu.Trigger>
		<DropdownMenu.Content color="gray" variant="soft">
			<DropdownMenu.Item shortcut="⌘ E">Edit</DropdownMenu.Item>
			<DropdownMenu.Item shortcut="⌘ D">Duplicate</DropdownMenu.Item>
			<DropdownMenu.Separator />
			<DropdownMenu.Item shortcut="⌘ N">Archive</DropdownMenu.Item>
		</DropdownMenu.Content>
	</DropdownMenu.Root>

	<DropdownMenu.Root>
		<DropdownMenu.Trigger>
			<Button color="gray" variant="soft" highContrast>
				Options
				<DropdownMenu.TriggerIcon />
			</Button>
		</DropdownMenu.Trigger>
		<DropdownMenu.Content color="gray" variant="soft" highContrast>
			<DropdownMenu.Item shortcut="⌘ E">Edit</DropdownMenu.Item>
			<DropdownMenu.Item shortcut="⌘ D">Duplicate</DropdownMenu.Item>
			<DropdownMenu.Separator />
			<DropdownMenu.Item shortcut="⌘ N">Archive</DropdownMenu.Item>
		</DropdownMenu.Content>
	</DropdownMenu.Root>
</Grid>
```

----------------------------------------

TITLE: Aligning Radio Group Items with Text in JSX
DESCRIPTION: Demonstrates how to compose RadioGroup.Item within Text components for automatic centering with the first line of text, including multiple size options.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/radio-group.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="3">
	<RadioGroup.Root size="1" defaultValue="1">
		<Text as="label" size="2">
			<Flex gap="2">
				<RadioGroup.Item value="1" /> Default
			</Flex>
		</Text>

		<Text as="label" size="2">
			<Flex gap="2">
				<RadioGroup.Item value="2" /> Compact
			</Flex>
		</Text>
	</RadioGroup.Root>

	<RadioGroup.Root size="2" defaultValue="1">
		<Text as="label" size="3">
			<Flex gap="2">
				<RadioGroup.Item value="1" /> Default
			</Flex>
		</Text>

		<Text as="label" size="3">
			<Flex gap="2">
				<RadioGroup.Item value="2" /> Compact
			</Flex>
		</Text>
	</RadioGroup.Root>

	<RadioGroup.Root size="3" defaultValue="1">
		<Text as="label" size="4">
			<Flex gap="2">
				<RadioGroup.Item value="1" /> Default
			</Flex>
		</Text>

		<Text as="label" size="4">
			<Flex gap="2">
				<RadioGroup.Item value="2" /> Compact
			</Flex>
		</Text>
	</RadioGroup.Root>
</Flex>
```

----------------------------------------

TITLE: Accessing Space Scale with CSS Variables in Radix UI
DESCRIPTION: CSS variables for accessing the 9-step space scale tokens in Radix UI. These variables can be used to style custom components to maintain consistency with the rest of the theme.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/spacing.mdx#2025-04-21_snippet_0

LANGUAGE: css
CODE:
```
/* Space scale */
var(--space-1);
var(--space-2);
var(--space-3);
var(--space-4);
var(--space-5);
var(--space-6);
var(--space-7);
var(--space-8);
var(--space-9);
```

----------------------------------------

TITLE: Controlling Card Size with 'size' Prop - JSX
DESCRIPTION: This snippet demonstrates how to control the size of the Radix UI Card component using the `size` prop.  Different sizes are applied to three cards, showcasing the effect on the overall appearance. This example also includes `Avatar` and `Text` components to illustrate the impact of card size on contained elements.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/card.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
"<Flex gap=\"3\" direction=\"column\">\n\t<Box width=\"350px\">\n\t	<Card size=\"1\">\n\t		<Flex gap=\"3\" align=\"center\">\n\t			<Avatar size=\"3\" radius=\"full\" fallback=\"T\" color=\"indigo\" />\n\t			<Box>\n\t				<Text as=\"div\" size=\"2\" weight=\"bold\">\n\t					Teodros Girmay\n\t				</Text>\n\t				<Text as=\"div\" size=\"2\" color=\"gray\">\n\t					Engineering\n\t				</Text>\n\t			</Box>\n\t		</Flex>\n\t	</Card>\n\t</Box>\n\n\t<Box width=\"400px\">\n\t	<Card size=\"2\">\n\t		<Flex gap=\"4\" align=\"center\">\n\t			<Avatar size=\"4\" radius=\"full\" fallback=\"T\" color=\"indigo\" />\n\t			<Box>\n\t				<Text as=\"div\" weight=\"bold\">\n\t					Teodros Girmay\n\t				</Text>\n\t				<Text as=\"div\" color=\"gray\">\n\t					Engineering\n\t				</Text>\n\t		</Box>\n\t	</Flex>\n\t	</Card>\n\t</Box>\n\n\t<Box width=\"500px\">\n\t	<Card size=\"3\">\n\t		<Flex gap=\"4\" align=\"center\">\n\t			<Avatar size=\"5\" radius=\"full\" fallback=\"T\" color=\"indigo\" />\n\t			<Box>\n\t				<Text as=\"div\" size=\"4\" weight=\"bold\">\n\t					Teodros Girmay\n\t				</Text>\n\t				<Text as=\"div\" size=\"4\" color=\"gray\">\n\t					Engineering\n\t				</Text>\n\t		</Box>\n\t	</Flex>\n\t	</Card>\n\t</Box>\n</Flex>"
```

----------------------------------------

TITLE: Creating a Basic Grid Layout with Radix UI
DESCRIPTION: This snippet demonstrates how to create a basic grid layout using the Radix UI Grid component. It sets the number of columns to 3, the gap between grid items to 3 units, and the height of each row to 64 pixels, repeated twice. The width is set to auto to take up available space.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/grid.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
"<Grid columns=\"3\" gap=\"3\" rows=\"repeat(2, 64px)\" width=\"auto\">\n\t<DecorativeBox />\n\t<DecorativeBox />\n\t<DecorativeBox />\n\t<DecorativeBox />\n\t<DecorativeBox />\n\t<DecorativeBox />\n</Grid>"
```

----------------------------------------

TITLE: Composing Radio Group Component in React
DESCRIPTION: Example of how to import and compose the Radio Group component parts in a React application.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/radio-group.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { RadioGroup } from "radix-ui";

export default () => (
	<RadioGroup.Root>
		<RadioGroup.Item>
			<RadioGroup.Indicator />
		</RadioGroup.Item>
	</RadioGroup.Root>
);
```

----------------------------------------

TITLE: Defining Button Radius in JSX
DESCRIPTION: This snippet illustrates the use of the 'radius' prop to define the button's corner radius. It shows buttons with 'none', 'large', and 'full' radius values. Inputs are JSX markup, and output is buttons with different corner radii.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/button.mdx#2025-04-21_snippet_6

LANGUAGE: jsx
CODE:
```
<Flex gap="3">
	<Button radius="none" variant="soft">
		Edit profile
	</Button>
	<Button radius="large" variant="soft">
		Edit profile
	</Button>
	<Button radius="full" variant="soft">
		Edit profile
	</Button>
</Flex>
```

----------------------------------------

TITLE: Imperative Toast API Implementation in React
DESCRIPTION: This snippet demonstrates how to create an imperative API for the Toast component using React's forwardRef and useImperativeHandle hooks. This allows for programmatically triggering the display of the toast, enabling use cases such as toast duplication.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/toast.mdx#2025-04-21_snippet_12

LANGUAGE: jsx
CODE:
```
// your-toast.jsx
import * as React from "react";
import { Toast as ToastPrimitive } from "radix-ui";

export const Toast = React.forwardRef((props, forwardedRef) => {
	const { children, ...toastProps } = props;
	const [count, setCount] = React.useState(0);

	React.useImperativeHandle(forwardedRef, () => ({
		publish: () => setCount((count) => count + 1),
	}));

	return (
		<>
			{Array.from({ length: count }).map((_, index) => (
				<ToastPrimitive.Root key={index} {...toastProps}>
					<ToastPrimitive.Description>{children}</ToastPrimitive.Description>
					<ToastPrimitive.Close>Dismiss</ToastPrimitive.Close>
				</ToastPrimitive.Root>
			))}
		</>
	);
});
```

----------------------------------------

TITLE: Alert Dialog with Size Variants
DESCRIPTION: This snippet demonstrates creating multiple Alert Dialogs with different sizes and corresponding button triggers. The size prop is utilized to adjust padding and border-radius, affecting the dialog's appearance.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/alert-dialog.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Flex gap="4" align="center">
	<AlertDialog.Root>
		<AlertDialog.Trigger>
			<Button variant="soft">Size 1</Button>
		</AlertDialog.Trigger>
		<AlertDialog.Content __size__="1" maxWidth="300px">
			<Text as="p" trim="both" size="1">
				The quick brown fox jumps over the lazy dog.
			</Text>
		</AlertDialog.Content>
	</AlertDialog.Root>

	<AlertDialog.Root>
		<AlertDialog.Trigger>
			<Button variant="soft">Size 2</Button>
		</AlertDialog.Trigger>
		<AlertDialog.Content __size__="2" maxWidth="400px">
			<Text as="p" trim="both" size="2">
				The quick brown fox jumps over the lazy dog.
			</Text>
		</AlertDialog.Content>
	</AlertDialog.Root>

	<AlertDialog.Root>
		<AlertDialog.Trigger>
			<Button variant="soft">Size 3</Button>
		</AlertDialog.Trigger>
		<AlertDialog.Content __size__="3" maxWidth="500px">
			<Text as="p" trim="both" size="3">
				The quick brown fox jumps over the lazy dog.
			</Text>
		</AlertDialog.Content>
	</AlertDialog.Root>

	<AlertDialog.Root>
		<AlertDialog.Trigger>
			<Button variant="soft">Size 4</Button>
		</AlertDialog.Trigger>
		<AlertDialog.Content __size__="4">
			<Text as="p" trim="both" size="4">
				The quick brown fox jumps over the lazy dog.
			</Text>
		</AlertDialog.Content>
	</AlertDialog.Root>
</Flex>
```

----------------------------------------

TITLE: Composing Formatting Components in JSX
DESCRIPTION: Shows how to compose formatting components like Em and Strong to add emphasis and signal importance in text.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/typography.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Text>
	The <Em>most</Em> important thing to remember is,{" "}
	<Strong>stay positive</Strong>.
</Text>
```

----------------------------------------

TITLE: Installing Radix UI Visually Hidden Component
DESCRIPTION: This command installs the `@radix-ui/react-visually-hidden` package from npm. This package provides the VisuallyHidden component for React applications, allowing developers to hide content visually while maintaining accessibility.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/utilities/visually-hidden.mdx#2025-04-21_snippet_0

LANGUAGE: bash
CODE:
```
```bash
npm install @radix-ui/react-visually-hidden
```
```

----------------------------------------

TITLE: CSS Keyframe Animations for Radix Primitives
DESCRIPTION: Define CSS keyframe animations for fading in and out Dialog components using state-based selectors. Supports both mount and unmount phase animations.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/guides/animation.mdx#2025-04-21_snippet_0

LANGUAGE: css
CODE:
```
@keyframes fadeIn {
	from {
		opacity: 0;
	}
	to {
		opacity: 1;
	}
}

@keyframes fadeOut {
	from {
		opacity: 1;
	}
	to {
		opacity: 0;
	}
}

.DialogOverlay[data-state="open"],
.DialogContent[data-state="open"] {
	animation: fadeIn 300ms ease-out;
}

.DialogOverlay[data-state="closed"],
.DialogContent[data-state="closed"] {
	animation: fadeOut 300ms ease-in;
}
```

----------------------------------------

TITLE: Switch Component with Different Variants in JSX
DESCRIPTION: Shows different visual styles of the Switch component using the variant prop with values 'surface', 'classic', and 'soft', in both unchecked and checked states.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/switch.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Flex gap="2">
	<Flex direction="column" gap="3">
		<Switch variant="surface" />
		<Switch variant="classic" />
		<Switch variant="soft" />
	</Flex>

	<Flex direction="column" gap="3">
		<Switch variant="surface" defaultChecked />
		<Switch variant="classic" defaultChecked />
		<Switch variant="soft" defaultChecked />
	</Flex>
</Flex>
```

----------------------------------------

TITLE: Form Element Reset Examples
DESCRIPTION: Examples of applying Reset to form-related elements like inputs, buttons, and textareas. These examples show how Reset handles elements that typically have strong browser defaults.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/reset.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Reset>
	<button>Button</button>
</Reset>
```

LANGUAGE: jsx
CODE:
```
<Reset>
	<input placeholder="Input control" />
</Reset>
```

LANGUAGE: jsx
CODE:
```
<Reset>
	<textarea placeholder="Text area" />
</Reset>
```

----------------------------------------

TITLE: Disabled Switch Component in JSX
DESCRIPTION: Demonstrates the Switch component in disabled states, both checked and unchecked, using the native disabled attribute.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/switch.mdx#2025-04-21_snippet_7

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="2">
	<Text as="label" size="2">
		<Flex gap="2">
			<Switch size="1" />
			Off
		</Flex>
	</Text>

	<Text as="label" size="2">
		<Flex gap="2">
			<Switch size="1" defaultChecked />
			On
		</Flex>
	</Text>

	<Text as="label" size="2" color="gray">
		<Flex gap="2">
			<Switch size="1" disabled />
			On
		</Flex>
	</Text>

	<Text as="label" size="2" color="gray">
		<Flex gap="2">
			<Switch size="1" disabled defaultChecked />
			Off
		</Flex>
	</Text>
</Flex>
```

----------------------------------------

TITLE: Aligning Radio Buttons with Text in JSX
DESCRIPTION: This example shows how to align radio buttons with text using the Flex and Text components. It demonstrates proper alignment with single-line and multi-line text for different sizes of radio buttons.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/radio.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="3">
	<Flex align="start" direction="column" gap="1">
		<Flex asChild gap="2">
			<Text as="label" size="2">
				<Radio size="1" name="alignment-1" value="1" defaultChecked />
				Default
			</Text>
		</Flex>
		<Flex asChild gap="2">
			<Text as="label" size="2">
				<Radio size="1" name="alignment-1" value="2" />
				Compact
			</Text>
		</Flex>
	</Flex>

	<Flex align="start" direction="column" gap="1">
		<Flex asChild gap="2">
			<Text as="label" size="3">
				<Radio size="2" name="alignment-2" value="1" defaultChecked />
				Default
			</Text>
		</Flex>
		<Flex asChild gap="2">
			<Text as="label" size="3">
				<Radio size="2" name="alignment-2" value="2" />
				Compact
			</Text>
		</Flex>
	</Flex>

	<Flex align="start" direction="column" gap="1">
		<Flex asChild gap="2">
			<Text as="label" size="4">
				<Radio size="3" name="alignment-3" value="1" defaultChecked />
				Default
			</Text>
		</Flex>
		<Flex asChild gap="2">
			<Text as="label" size="4">
				<Radio size="3" name="alignment-3" value="2" />
				Compact
			</Text>
		</Flex>
	</Flex>
</Flex>
```

----------------------------------------

TITLE: Card-Style Components with Leading Trim in JSX
DESCRIPTION: Example showing how trimming the leading space is useful for card or boxed components to create visually balanced spacing between text and container edges.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/text.mdx#2025-04-21_snippet_8

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="3">
	<Box
		style={{
			background: "var(--gray-a2)",
			border: "1px dashed var(--gray-a7)",
		}}
		p="4"
	>
		<Heading mb="1" size="3">
			Without trim
		</Heading>
		<Text>
			The goal of typography is to relate font size, line height, and line width
			in a proportional way that maximizes beauty and makes reading easier and
			more pleasant.
		</Text>
	</Box>

	<Box
		p="4"
		style={{
			background: "var(--gray-a2)",
			border: "1px dashed var(--gray-a7)",
		}}
	>
		<Heading mb="1" size="3" trim="start">
			With trim
		</Heading>
		<Text trim="end">
			The goal of typography is to relate font size, line height, and line width
			in a proportional way that maximizes beauty and makes reading easier and
			more pleasant.
		</Text>
	</Box>
</Flex>
```

----------------------------------------

TITLE: Using Radix Colors with styled-components
DESCRIPTION: This snippet demonstrates how to use Radix Colors with styled-components. It imports color scales, creates light and dark themes, and applies the colors to a styled button component. Requires `styled-components` and `@radix-ui/colors` packages.
SOURCE: https://github.com/radix-ui/website/blob/main/data/colors/docs/overview/usage.mdx#2025-04-21_snippet_2

LANGUAGE: javascript
CODE:
```
import {
	gray,
	blue,
	red,
	green,
	grayDark,
	blueDark,
	redDark,
	greenDark,
} from "@radix-ui/colors";
import styled, { ThemeProvider } from "styled-components";

// Create your theme
const theme = {
	colors: {
		...gray,
		...blue,
		...red,
		...green,
	},
};

// Create your dark theme
const darkTheme = {
	colors: {
		...grayDark,
		...blueDark,
		...redDark,
		...greenDark,
	},
};

// Use the colors in your styles
const Button = styled.button`
	background-color: ${(props) => props.theme.colors.blue4};
	color: ${(props) => props.theme.colors.blue11};
	border-color: ${(props) => props.theme.colors.blue7};
	&:hover {
		background-color: ${(props) => props.theme.colors.blue5};
		border-color: ${(props) => props.theme.colors.blue8};
	}
`;

// Wrap your app with the theme provider and apply your theme to it
export default function App() {
	return (
		<ThemeProvider theme={theme}>
			<Button>Radix Colors</Button>
		</ThemeProvider>
	);
}
```

----------------------------------------

TITLE: Rendering Simple Code Snippet Using JSX
DESCRIPTION: This snippet demonstrates the basic rendering of a code fragment using the 'Code' component in JSX. It shows an example of logging to the console.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/code.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Code>console.log()</Code>
```

----------------------------------------

TITLE: Custom Validation Message in Form
DESCRIPTION: Demonstrates how to provide a custom validation message for a specific validation condition.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/form.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
<Form.Message match="valueMissing">__Please provide a name__</Form.Message>
```

----------------------------------------

TITLE: Utilizing Inset Content within a Hover Card in React
DESCRIPTION: This snippet demonstrates the use of the Inset component to align content flush to the sides of the Hover Card, providing a more appealing layout. It showcases an image and text within the Hover Card, enhancing the design by controlling the layout and alignment of the displayed content.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/hover-card.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
"<Text>\n\tTechnology revolutionized{\" \\n\t<HoverCard.Root>\n\t\t<HoverCard.Trigger>\n\t\t\t<Link href=\"#\">typography</Link>\n\t\t</HoverCard.Trigger>\n\t\t<HoverCard.Content width=\"450px\">\n\t\t\t<Flex>\n\t\t\t\t<Box asChild flexShrink=\"0\">\n\t\t\t\t\t<Inset side=\"left\" pr=\"current\">\n\t\t\t\t\t\t<img\n\t\t\t\t\t\tsrc=\"https://images.unsplash.com/photo-1617050318658-a9a3175e34cb?&auto=format&fit=crop&w=300&q=90\"\n\t\t\t\t\t\t\talt=\"Bold typography\"\n\t\t\t\t\t\t\tstyle={{\n\t\t\t\t\t\t\tdisplay: \"block\",\n\t\t\t\t\t\t\tobjectFit: \"cover\",\n\t\t\t\t\t\t\theight: \"100%\",\n\t\t\t\t\t\t\twidth: 150,\n\t\t\t\t\t\t\tbackgroundColor: \"var(--gray-5)\"\n\t\t\t\t\t\t}}/>{\" \\n\t\t\t\t\t</Inset>\n\t\t\t\t</Box>\n\t\t\t\t<Text size=\"2\" as=\"p\">\n\t\t\t\t\t<Strong>Typography</Strong> is the art and technique of arranging type\n\t\t\t\t\tto make written language legible, readable and appealing when\n\t\t\t\t\tdisplayed. The arrangement of type involves selecting typefaces, point\n\t\t\t\t\tsizes, line lengths, line-spacing (leading), and letter-spacing\n\t\t\t\t\t(tracking)…\n\t\t\t\t</Text>\n\t\t\t</Flex>\n\t\t</HoverCard.Content>\n\t</HoverCard.Root>{\" \\n\tin the latter twentieth century.\n\t</Text>"
```

----------------------------------------

TITLE: Dialog Size Variations in Radix UI
DESCRIPTION: This snippet illustrates how to implement multiple size variations for the dialog component using Radix UI. It demonstrates creating multiple dialog instances with different size properties, allowing control over visual aspects like padding and width.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/dialog.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Flex gap="4" align="center">
	<Dialog.Root>
		<Dialog.Trigger>
			<Button variant="soft">Size 1</Button>
		</Dialog.Trigger>
		<Dialog.Content __size__="1" maxWidth="300px">
			<Text as="p" trim="both" size="1">
				The quick brown fox jumps over the lazy dog.
			</Text>
		</Dialog.Content>
	</Dialog.Root>

	<Dialog.Root>
		<Dialog.Trigger>
			<Button variant="soft">Size 2</Button>
		</Dialog.Trigger>
		<Dialog.Content __size__="2" maxWidth="400px">
			<Text as="p" trim="both" size="2">
				The quick brown fox jumps over the lazy dog.
			</Text>
		</Dialog.Content>
	</Dialog.Root>

	<Dialog.Root>
		<Dialog.Trigger>
			<Button variant="soft">Size 3</Button>
		</Dialog.Trigger>
		<Dialog.Content __size__="3" maxWidth="500px">
			<Text as="p" trim="both" size="3">
				The quick brown fox jumps over the lazy dog.
			</Text>
		</Dialog.Content>
	</Dialog.Root>

	<Dialog.Root>
		<Dialog.Trigger>
			<Button variant="soft">Size 4</Button>
		</Dialog.Trigger>
		<Dialog.Content __size__="4">
			<Text as="p" trim="both" size="4">
				The quick brown fox jumps over the lazy dog.
			</Text>
		</Dialog.Content>
	</Dialog.Root>
</Flex>
```

----------------------------------------

TITLE: Styling Card Variant with 'variant' Prop - JSX
DESCRIPTION: This snippet demonstrates how to control the visual style of the Radix UI Card component using the `variant` prop. Two cards are displayed with different variants (`surface` and `classic`), showcasing the available styling options. This allows for creating visually distinct cards to suit different design requirements.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/card.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
"<Flex direction=\"column\" gap=\"3\" maxWidth=\"350px\">\n\t<Card variant=\"surface\">\n\t	<Text as=\"div\" size=\"2\" weight=\"bold\">\n\t		Quick start\n\t	</Text>\n\t	<Text as=\"div\" color=\"gray\" size=\"2\">\n\t		Start building your next project in minutes\n\t	</Text>\n\t</Card>\n\n\t<Card variant=\"classic\">\n\t	<Text as=\"div\" size=\"2\" weight=\"bold\">\n\t		Quick start\n\t	</Text>\n\t	<Text as=\"div\" color=\"gray\" size=\"2\">\n\t		Start building your next project in minutes\n\t	</Text>\n\t</Card>\n</Flex>"
```

----------------------------------------

TITLE: Disabled Checkbox Cards in React
DESCRIPTION: Illustrates how to create disabled checkbox card items that cannot be interacted with
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/checkbox-cards.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="4" maxWidth="450px">
	<CheckboxCards.Root columns="2" defaultValue="2">
		<CheckboxCards.Item value="1">Off</CheckboxCards.Item>
		<CheckboxCards.Item value="2">On</CheckboxCards.Item>
	</CheckboxCards.Root>

	<CheckboxCards.Root columns="2" defaultValue="2">
		<CheckboxCards.Item value="1" disabled>
			Off
		</CheckboxCards.Item>
		<CheckboxCards.Item value="2" disabled>
			On
		</CheckboxCards.Item>
	</CheckboxCards.Root>
</Flex>
```

----------------------------------------

TITLE: Configuring Global Tooltip Provider in React
DESCRIPTION: Demonstrates how to use Tooltip.Provider to set global delay configurations for multiple tooltip instances across an application
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/tooltip.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
import { Tooltip } from "radix-ui";

export default () => (
	<Tooltip.Provider __delayDuration__={800} __skipDelayDuration__={500}>
		<Tooltip.Root>
			<Tooltip.Trigger>…</Tooltip.Trigger>
			<Tooltip.Content>…</Tooltip.Content>
		</Tooltip.Root>
		<Tooltip.Root>
			<Tooltip.Trigger>…</Tooltip.Trigger>
			<Tooltip.Content>…</Tooltip.Content>
		</Tooltip.Root>
	</Tooltip.Provider>
);
```

----------------------------------------

TITLE: Initializing Radix UI Select with Popper Positioning
DESCRIPTION: Demonstrates how to configure Select component with alternative positioning mode using popper strategy and side offset
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/select.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
import { Select } from "radix-ui";

export default () => (
	<Select.Root>
		<Select.Trigger>…</Select.Trigger>
		<Select.Portal>
			<Select.Content __position__="popper" __sideOffset__={5}>
				…
			</Select.Content>
		</Select.Portal>
	</Select.Root>
);
```

----------------------------------------

TITLE: Styling Text with Slate Color in JSX
DESCRIPTION: This snippet shows how to style text using the Slate color variable, demonstrating a practical example of using Radix Colors to achieve a harmonious and functional text appearance in JSX components. Presuming a React-based setup, it depends on the Radix color system being integrated as CSS variables.
SOURCE: https://github.com/radix-ui/website/blob/main/data/colors/docs/palette-composition/composing-a-palette.mdx#2025-04-21_snippet_1

LANGUAGE: JSX
CODE:
```
<Box my="5">
	<Text
		as="p"
		size="7"
		align="center"
		weight="bold"
		style={{ color: "var(--slate-12)" }}
	>
		This text is Slate 12
	</Text>
</Box>
```

----------------------------------------

TITLE: Switch Component Aligned with Text in JSX
DESCRIPTION: Shows how the Switch component aligns with different sizes of text when composed within Text components, automatically centering with the first line of text.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/switch.mdx#2025-04-21_snippet_6

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="3">
	<Text as="label" size="2">
		<Flex gap="2">
			<Switch size="1" defaultChecked /> Sync settings
		</Flex>
	</Text>

	<Text as="label" size="3">
		<Flex gap="2">
			<Switch size="2" defaultChecked /> Sync settings
		</Flex>
	</Text>

	<Text as="label" size="4">
		<Flex gap="2">
			<Switch size="3" defaultChecked /> Sync settings
		</Flex>
	</Text>
</Flex>
```

----------------------------------------

TITLE: Composing Radix UI Accordion Component
DESCRIPTION: Example of how to import and compose the Accordion component parts.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/accordion.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { Accordion } from "radix-ui";

export default () => (
	<Accordion.Root>
		<Accordion.Item>
			<Accordion.Header>
				<Accordion.Trigger />
			</Accordion.Header>
			<Accordion.Content />
		</Accordion.Item>
	</Accordion.Root>
);
```

----------------------------------------

TITLE: Displaying a Basic Callout Component in React
DESCRIPTION: This snippet shows the basic structure of a callout component using Radix UI. It includes an icon and text to deliver a message. The `Callout.Root` acts as the container with `Callout.Icon` and `Callout.Text` as children. No additional props are specified, so default styles are applied. Prerequisites include the installation of Radix UI components and icons like `InfoCircledIcon`.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/callout.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Callout.Root>\n\t<Callout.Icon>\n\t\t<InfoCircledIcon />\n\t</Callout.Icon>\n\t<Callout.Text>\n\t\tYou will need admin privileges to install and access this application.\n\t</Callout.Text>\n</Callout.Root>
```

----------------------------------------

TITLE: Setting Color for Checkbox Group - React
DESCRIPTION: This snippet demonstrates how to use the `color` prop to assign distinctive colors to each Checkbox Group component. Various color options are illustrated, enhancing the visual appeal according to specified themes.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/checkbox-group.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Flex gap="2">
	<CheckboxGroup.Root color="indigo" defaultValue="1">
		<CheckboxGroup.Item value="1" />
	</CheckboxGroup.Root>

	<CheckboxGroup.Root color="cyan" defaultValue="1">
		<CheckboxGroup.Item value="1" />
	</CheckboxGroup.Root>

	<CheckboxGroup.Root color="orange" defaultValue="1">
		<CheckboxGroup.Item value="1" />
	</CheckboxGroup.Root>

	<CheckboxGroup.Root color="crimson" defaultValue="1">
		<CheckboxGroup.Item value="1" />
	</CheckboxGroup.Root>
</Flex>
```

----------------------------------------

TITLE: Customizing Callout Variant in React
DESCRIPTION: Demonstrates changing the visual style of callout components using the `variant` prop in Radix UI. Different variant values such as `soft`, `surface`, and `outline` offer distinct appearances. This feature allows developers to align callouts with the overall design theme of the application. Requires Radix UI library and compatible styling components.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/callout.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Flex direction=\"column\" gap=\"3\">\n\t<Callout.Root variant=\"soft\">\n\t\t<Callout.Icon>\n\t\t\t<InfoCircledIcon />\n\t\t</Callout.Icon>\n\t\t<Callout.Text>\n\t\t\tYou will need <Link href=\"#\">admin privileges</Link> to install and access\n\t\t\tthis application.\n\t\t</Callout.Text>\n\t</Callout.Root>\n\n\t<Callout.Root variant=\"surface\">\n\t\t<Callout.Icon>\n\t\t\t<InfoCircledIcon />\n\t\t</Callout.Icon>\n\t\t<Callout.Text>\n\t\t\tYou will need <Link href=\"#\">admin privileges</Link> to install and access\n\t\t\tthis application.\n\t\t</Callout.Text>\n\t</Callout.Root>\n\n\t<Callout.Root variant=\"outline\">\n\t\t<Callout.Icon>\n\t\t\t<InfoCircledIcon />\n\t\t</Callout.Icon>\n\t\t<Callout.Text>\n\t\t\tYou will need <Link href=\"#\">admin privileges</Link> to install and access\n\t\t\tthis application.\n\t\t</Callout.Text>\n\t</Callout.Root>\n</Flex>
```

----------------------------------------

TITLE: Creating Rotating Icon in Accordion with React
DESCRIPTION: This snippet demonstrates the use of Radix UI's Accordion component within a React functional component. It integrates a rotating chevron icon to indicate the state of the accordion item (open or closed). Required dependencies include 'radix-ui' for the accordion component and '@radix-ui/react-icons' for the icon. The expected output is an interactive accordion with a visually indicating chevron icon.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/accordion.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
// index.jsx
import { Accordion } from "radix-ui";
import { ChevronDownIcon } from "@radix-ui/react-icons";
import "./styles.css";

export default () => (
	<Accordion.Root type="single">
		<Accordion.Item value="item-1">
			<Accordion.Header>
				<Accordion.Trigger className="AccordionTrigger">
					<span>Trigger text</span>
					<ChevronDownIcon __className__="AccordionChevron" aria-hidden />
				</Accordion.Trigger>
			</Accordion.Header>
			<Accordion.Content>…</Accordion.Content>
		</Accordion.Item>
	</Accordion.Root>
);
```

----------------------------------------

TITLE: Size-Controlled Skeleton Component
DESCRIPTION: Example showing how to manually control Skeleton dimensions using width and height props.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/skeleton.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Skeleton width="48px" height="48px" />
```

----------------------------------------

TITLE: Basic Collapsible Component Structure
DESCRIPTION: Basic implementation showing the main structure of the Collapsible component with Root, Trigger, and Content parts.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/collapsible.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { Collapsible } from "radix-ui";

export default () => (
	<Collapsible.Root>
		<Collapsible.Trigger />
		<Collapsible.Content />
	</Collapsible.Root>
);
```

----------------------------------------

TITLE: Checkbox Card Color Customization in React
DESCRIPTION: Shows how to apply different color themes to checkbox cards using the color prop
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/checkbox-cards.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="3" maxWidth="200px">
	<CheckboxCards.Root defaultValue={["1"]} color="indigo">
		<CheckboxCards.Item value="1">Agree to Terms</CheckboxCards.Item>
	</CheckboxCards.Root>

	<CheckboxCards.Root defaultValue={["1"]} color="cyan">
		<CheckboxCards.Item value="1">Agree to Terms</CheckboxCards.Item>
	</CheckboxCards.Root>

	<CheckboxCards.Root defaultValue={["1"]} color="orange">
		<CheckboxCards.Item value="1">Agree to Terms</CheckboxCards.Item>
	</CheckboxCards.Root>

	<CheckboxCards.Root defaultValue={["1"]} color="crimson">
		<CheckboxCards.Item value="1">Agree to Terms</CheckboxCards.Item>
	</CheckboxCards.Root>
</Flex>
```

----------------------------------------

TITLE: Using Color Prop to Define Dropdown Menu Colors
DESCRIPTION: This snippet shows how to utilize the `color` prop to specify a color scheme for the dropdown menu and its items. It provides multiple examples of using different colors for both the trigger button and dropdown content.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/dropdown-menu.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Flex gap="3">
	<DropdownMenu.Root>
		<DropdownMenu.Trigger>
			<Button variant="soft" color="indigo">
				Options
				<DropdownMenu.TriggerIcon />
			</Button>
		</DropdownMenu.Trigger>
		<DropdownMenu.Content variant="soft" color="indigo">
			<DropdownMenu.Item shortcut="⌘ E">Edit</DropdownMenu.Item>
			<DropdownMenu.Item shortcut="⌘ D">Duplicate</DropdownMenu.Item>
			<DropdownMenu.Separator />
			<DropdownMenu.Item shortcut="⌘ N">Archive</DropdownMenu.Item>
		</DropdownMenu.Content>
	</DropdownMenu.Root>

	<DropdownMenu.Root>
		<DropdownMenu.Trigger>
			<Button variant="soft" color="cyan">
				Options
				<DropdownMenu.TriggerIcon />
			</Button>
		</DropdownMenu.Trigger>
		<DropdownMenu.Content variant="soft" color="cyan">
			<DropdownMenu.Item shortcut="⌘ E">Edit</DropdownMenu.Item>
			<DropdownMenu.Item shortcut="⌘ D">Duplicate</DropdownMenu.Item>
			<DropdownMenu.Separator />
			<DropdownMenu.Item shortcut="⌘ N">Archive</DropdownMenu.Item>
		</DropdownMenu.Content>
	</DropdownMenu.Root>

	<DropdownMenu.Root>
		<DropdownMenu.Trigger>
			<Button variant="soft" color="orange">
				Options
				<DropdownMenu.TriggerIcon />
			</Button>
		</DropdownMenu.Trigger>
		<DropdownMenu.Content variant="soft" color="orange">
			<DropdownMenu.Item shortcut="⌘ E">Edit</DropdownMenu.Item>
			<DropdownMenu.Item shortcut="⌘ D">Duplicate</DropdownMenu.Item>
			<DropdownMenu.Separator />
			<DropdownMenu.Item shortcut="⌘ N">Archive</DropdownMenu.Item>
		</DropdownMenu.Content>
	</DropdownMenu.Root>

	<DropdownMenu.Root>
		<DropdownMenu.Trigger>
			<Button variant="soft" color="crimson">
				Options
				<DropdownMenu.TriggerIcon />
			</Button>
		</DropdownMenu.Trigger>
		<DropdownMenu.Content variant="soft" color="crimson">
			<DropdownMenu.Item shortcut="⌘ E">Edit</DropdownMenu.Item>
			<DropdownMenu.Item shortcut="⌘ D">Duplicate</DropdownMenu.Item>
			<DropdownMenu.Separator />
			<DropdownMenu.Item shortcut="⌘ N">Archive</DropdownMenu.Item>
		</DropdownMenu.Content>
	</DropdownMenu.Root>
</Flex>
```

----------------------------------------

TITLE: Checkbox Alignment in Text with Flex in JSX
DESCRIPTION: Demonstrates how to automatically center a Checkbox component within text using Flex in JSX. Various sizes of Text and Checkbox components are used to illustrate alignment capabilities with multi-line text support as well.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/checkbox.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="3">
	<Text as="label" size="2">
		<Flex as="span" gap="2">
			<Checkbox size="1" defaultChecked /> Agree to Terms and Conditions
		</Flex>
	</Text>

	<Text as="label" size="3">
		<Flex as="span" gap="2">
			<Checkbox size="2" defaultChecked /> Agree to Terms and Conditions
		</Flex>
	</Text>

	<Text as="label" size="4">
		<Flex as="span" gap="2">
			<Checkbox size="3" defaultChecked /> Agree to Terms and Conditions
		</Flex>
	</Text>
</Flex>
```

LANGUAGE: jsx
CODE:
```
<Box maxWidth="300px">
	<Text as="label" size="3">
		<Flex as="span" gap="2">
			<Checkbox defaultChecked /> I understand that these documents are
			confidential and cannot be shared with a third party.
		</Flex>
	</Text>
</Box>
```

----------------------------------------

TITLE: Basic Usage of One-Time Password Field with Multiple Inputs
DESCRIPTION: Illustrates rendering a One-Time Password Field component with six separate Input components to accommodate a 6-character password. It includes mapping over the desired password length to generate Input elements and a HiddenInput for data management.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/one-time-password-field.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
// This will render a field with 6 inputs, for use with
// 6-character passwords. Render an Input component for
// each character of accepted password's length.
<OneTimePasswordField.Root>
	<OneTimePasswordField.Input />
	<OneTimePasswordField.Input />
	<OneTimePasswordField.Input />
	<OneTimePasswordField.Input />
	<OneTimePasswordField.Input />
	<OneTimePasswordField.Input />
	<OneTimePasswordField.HiddenInput />
</OneTimePasswordField.Root>
```

----------------------------------------

TITLE: Displaying Content with Aspect Ratio in React
DESCRIPTION: This JSX snippet demonstrates how to use the `AspectRatio` component from Radix UI to display an image with a specified aspect ratio of 16:8. The `ratio` prop controls the aspect ratio, and the `img` element's styles ensure it covers the container while maintaining the specified ratio.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/aspect-ratio.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
"<AspectRatio ratio={16 / 8}>\n\t<img\n\t	src=\"https://images.unsplash.com/photo-1479030160180-b1860951d696?&auto=format&fit=crop&w=1200&q=80\"\n\t	alt=\"A house in a forest\"\n\t	style={{\n\t		objectFit: \"cover\",\n\t		width: \"100%\",\n\t		height: \"100%\",\n\t		borderRadius: \"var(--radius-2)\",\n\t	}}\n\t/>\n</AspectRatio>"
```

----------------------------------------

TITLE: Customizing Table Size in Radix UI
DESCRIPTION: Example demonstrating the use of the size prop to control text size and cell padding in Radix UI tables. Three different size variations (1, 2, and 3) are shown for comparison.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/table.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="5" maxWidth="350px">
	<Table.Root __size__="1">
		<Table.Header>
			<Table.Row>
				<Table.ColumnHeaderCell>Full name</Table.ColumnHeaderCell>
				<Table.ColumnHeaderCell>Email</Table.ColumnHeaderCell>
			</Table.Row>
		</Table.Header>

		<Table.Body>
			<Table.Row>
				<Table.RowHeaderCell>Danilo Sousa</Table.RowHeaderCell>
				<Table.Cell>danilo@example.com</Table.Cell>
			</Table.Row>
			<Table.Row>
				<Table.RowHeaderCell>Zahra Ambessa</Table.RowHeaderCell>
				<Table.Cell>zahra@example.com</Table.Cell>
			</Table.Row>
		</Table.Body>
	</Table.Root>

	<Table.Root __size__="2">
		<Table.Header>
			<Table.Row>
				<Table.ColumnHeaderCell>Full name</Table.ColumnHeaderCell>
				<Table.ColumnHeaderCell>Email</Table.ColumnHeaderCell>
			</Table.Row>
		</Table.Header>

		<Table.Body>
			<Table.Row>
				<Table.RowHeaderCell>Danilo Sousa</Table.RowHeaderCell>
				<Table.Cell>danilo@example.com</Table.Cell>
			</Table.Row>
			<Table.Row>
				<Table.RowHeaderCell>Zahra Ambessa</Table.RowHeaderCell>
				<Table.Cell>zahra@example.com</Table.Cell>
			</Table.Row>
		</Table.Body>
	</Table.Root>

	<Table.Root __size__="3">
		<Table.Header>
			<Table.Row>
				<Table.ColumnHeaderCell>Full name</Table.ColumnHeaderCell>
				<Table.ColumnHeaderCell>Email</Table.ColumnHeaderCell>
			</Table.Row>
		</Table.Header>

		<Table.Body>
			<Table.Row>
				<Table.RowHeaderCell>Danilo Sousa</Table.RowHeaderCell>
				<Table.Cell>danilo@example.com</Table.Cell>
			</Table.Row>
			<Table.Row>
				<Table.RowHeaderCell>Zahra Ambessa</Table.RowHeaderCell>
				<Table.Cell>zahra@example.com</Table.Cell>
			</Table.Row>
		</Table.Body>
	</Table.Root>
</Flex>
```

----------------------------------------

TITLE: Configuring Theme Radius in JSX
DESCRIPTION: Demonstrates how to set the radius theme for a group of components using the Theme component with the radius prop.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/radius.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Theme radius="medium">
	<TextField.Root size="3" placeholder="Reply…">
		<TextField.Slot side="right" px="1">
			<Button size="2">Send</Button>
		</TextField.Slot>
	</TextField.Root>
</Theme>
```

----------------------------------------

TITLE: Applying Color to Callouts in React
DESCRIPTION: Illustrates the usage of the `color` prop to assign specific colors to callout components. Various color themes such as `blue`, `green`, and `red` can be used to fit the application's visual identity. This customization leverages Radix UI's theming capabilities. Dependencies include Radix UI components and appropriate CSS for color application.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/callout.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Flex direction=\"column\" gap=\"3\">\n\t<Callout.Root color=\"blue\">\n\t\t<Callout.Icon>\n\t\t\t<InfoCircledIcon />\n\t\t</Callout.Icon>\n\t\t<Callout.Text>\n\t\t\tYou will need <Link href=\"#\">admin privileges</Link> to install and access\n\t\t\tthis application.\n\t\t</Callout.Text>\n\t</Callout.Root>\n\n\t<Callout.Root color=\"green\">\n\t\t<Callout.Icon>\n\t\t\t<InfoCircledIcon />\n\t\t</Callout.Icon>\n\t\t<Callout.Text>\n\t\t\tYou will need <Link href=\"#\">admin privileges</Link> to install and access\n\t\t\tthis application.\n\t\t</Callout.Text>\n\t</Callout.Root>\n\n\t<Callout.Root color=\"red\">\n\t\t<Callout.Icon>\n\t\t\t<InfoCircledIcon />\n\t\t</Callout.Icon>\n\t\t<Callout.Text>\n\t\t\tYou will need <Link href=\"#\">admin privileges</Link> to install and access\n\t\t\tthis application.\n\t\t</Callout.Text>\n\t</Callout.Root>\n</Flex>
```

----------------------------------------

TITLE: Styling Popover Component in JSX
DESCRIPTION: This JSX snippet illustrates how to add custom CSS class names to the Popover component elements, allowing for personalized styling of the Popover.Trigger, Popover.Content, and Popover.Arrow.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/overview/getting-started.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
// index.jsx
import * as React from "react";
import { Popover } from "radix-ui";
import "./styles.css";

const PopoverDemo = () => (
	<Popover.Root>
		<Popover.Trigger __className__="PopoverTrigger">Show info</Popover.Trigger>
		<Popover.Portal>
			<Popover.Content __className__="PopoverContent">
				Some content
				<Popover.Arrow __className__="PopoverArrow" />
			</Popover.Content>
		</Popover.Portal>
	</Popover.Root>
);

export default PopoverDemo;
```

----------------------------------------

TITLE: Implementing Size Variations for Hover Cards in React
DESCRIPTION: This snippet illustrates how to create multiple Hover Cards with different size variations, showcasing the flexibility of the Hover Card component in displaying text with varying maximum widths. Each card has a different size prop and displays similar content anatomy.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/hover-card.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
"<Flex gap=\"4\">\n\t<HoverCard.Root>\n\t\t<HoverCard.Trigger>\n\t\t\t<Link href=\"#\">Size 1</Link>\n\t\t</HoverCard.Trigger>\n\t\t<HoverCard.Content size=\"1\" maxWidth=\"240px\">\n\t\t\t<Text as=\"div\" size=\"1\" trim=\"both\">\n\t\t\t\t<Strong>Typography</Strong> is the art and technique of arranging type\n\t\t\t\tto make written language legible, readable and appealing when displayed.\n\t\t\t</Text>\n\t\t</HoverCard.Content>\n\t</HoverCard.Root>\n\n\t<HoverCard.Root>\n\t\t<HoverCard.Trigger>\n\t\t\t<Link href=\"#\">Size 2</Link>\n\t\t</HoverCard.Trigger>\n\t\t<HoverCard.Content size=\"2\" maxWidth=\"280px\">\n\t\t\t<Text as=\"div\" size=\"2\" trim=\"both\">\n\t\t\t\t<Strong>Typography</Strong> is the art and technique of arranging type\n\t\t\t\tto make written language legible, readable and appealing when displayed.\n\t\t\t</Text>\n\t\t</HoverCard.Content>\n\t</HoverCard.Root>\n\n\t<HoverCard.Root>\n\t\t<HoverCard.Trigger>\n\t\t\t<Link href=\"#\">Size 3</Link>\n\t\t</HoverCard.Trigger>\n\t\t<HoverCard.Content size=\"3\" maxWidth=\"320px\">\n\t\t\t<Text as=\"div\" size=\"3\" trim=\"both\">\n\t\t\t\t<Strong>Typography</Strong> is the art and technique of arranging type\n\t\t\t\tto make written language legible, readable and appealing when displayed.\n\t\t\t</Text>\n\t\t</HoverCard.Content>\n\t</HoverCard.Root>\n</Flex>"
```

----------------------------------------

TITLE: Origin-Aware Tooltip Animations
DESCRIPTION: Utilizes Radix UI's transform-origin CSS variable to create dynamic, origin-aware scale animations for tooltips
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/tooltip.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
// index.jsx
import { Tooltip } from "radix-ui";
import "./styles.css";

export default () => (
	<Tooltip.Root>
		<Tooltip.Trigger>…</Tooltip.Trigger>
		<Tooltip.Content __className__="TooltipContent">…</Tooltip.Content>
	</Tooltip.Root>
);
```

LANGUAGE: css
CODE:
```
/* styles.css */
.TooltipContent {
	transform-origin: var(--radix-tooltip-content-transform-origin);
	animation: scaleIn 0.5s ease-out;
}

@keyframes scaleIn {
	from {
		opacity: 0;
		transform: scale(0);
	}
	to {
		opacity: 1;
		transform: scale(1);
	}
}
```

----------------------------------------

TITLE: Checkbox Card Visual Variants in React
DESCRIPTION: Demonstrates different visual styles for checkbox cards using the variant prop
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/checkbox-cards.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="3" maxWidth="200px">
	<CheckboxCards.Root defaultValue={["1"]} variant="surface">
		<CheckboxCards.Item value="1">Agree to Terms</CheckboxCards.Item>
	</CheckboxCards.Root>

	<CheckboxCards.Root defaultValue={["1"]} variant="classic">
		<CheckboxCards.Item value="1">Agree to Terms</CheckboxCards.Item>
	</CheckboxCards.Root>
</Flex>
```

----------------------------------------

TITLE: Applying Visual Variants to Checkbox Group - React
DESCRIPTION: This snippet illustrates the usage of the `variant` prop to alter the visual styles of the Checkbox Group. It contains examples of multiple variants being applied to achieve different visual appearances.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/checkbox-group.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Flex gap="2">
	<Flex direction="column" asChild gap="2">
		<CheckboxGroup.Root variant="surface" defaultValue="1">
			<CheckboxGroup.Item value="1" />
			<CheckboxGroup.Item value="2" />
		</CheckboxGroup.Root>
	</Flex>

	<Flex direction="column" asChild gap="2">
		<CheckboxGroup.Root variant="classic" defaultValue="1">
			<CheckboxGroup.Item value="1" />
			<CheckboxGroup.Item value="2" />
		</CheckboxGroup.Root>
	</Flex>

	<Flex direction="column" asChild gap="2">
		<CheckboxGroup.Root variant="soft" defaultValue="1">
			<CheckboxGroup.Item value="1" />
			<CheckboxGroup.Item value="2" />
		</CheckboxGroup.Root>
	</Flex>
</Flex>
```

----------------------------------------

TITLE: Implementing ContextMenu with Submenus in React
DESCRIPTION: Example of creating submenus in a ContextMenu component using ContextMenu.Sub and its related parts to create a nested menu structure.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/context-menu.mdx#2025-04-21_snippet_11

LANGUAGE: jsx
CODE:
```
<ContextMenu.Root>
	<ContextMenu.Trigger>…</ContextMenu.Trigger>
	<ContextMenu.Portal>
		<ContextMenu.Content>
			<ContextMenu.Item>…</ContextMenu.Item>
			<ContextMenu.Item>…</ContextMenu.Item>
			<ContextMenu.Separator />
			<ContextMenu.Sub>
				<ContextMenu.SubTrigger>Sub menu →</ContextMenu.SubTrigger>
				<ContextMenu.Portal>
					<ContextMenu.SubContent>
						<ContextMenu.Item>Sub menu item</ContextMenu.Item>
						<ContextMenu.Item>Sub menu item</ContextMenu.Item>
						<ContextMenu.Arrow />
					</ContextMenu.SubContent>
				</ContextMenu.Portal>
			</ContextMenu.Sub>
			<ContextMenu.Separator />
			<ContextMenu.Item>…</ContextMenu.Item>
		</ContextMenu.Content>
	</ContextMenu.Portal>
</ContextMenu.Root>
```

----------------------------------------

TITLE: Custom Paragraph Style Example
DESCRIPTION: Example of CSS and JSX code showing how to create and apply custom paragraph styles with Radix Themes Box component using asChild prop.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/overview/styling.mdx#2025-04-21_snippet_2

LANGUAGE: css
CODE:
```
.my-paragraph {
	margin: 0;
}
```

LANGUAGE: jsx
CODE:
```
function MyApp() {
	return (
		<Theme>
			<Box asChild m="5">
				<p className="my-paragraph">My custom paragraph</p>
			</Box>
		</Theme>
	);
}
```

----------------------------------------

TITLE: Using next/font with Radix Themes in JSX
DESCRIPTION: Shows how to load and apply custom fonts using next/font in a Next.js application with Radix Themes.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/typography.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
import { Inter } from "next/font/google";

const inter = Inter({
	subsets: ["latin"],
	display: "swap",
	variable: "--font-inter",
});

export default function RootLayout({ children }) {
	return (
		<html lang="en" className={inter.variable}>
			<body>{children}</body>
		</html>
	);
}
```

----------------------------------------

TITLE: Defining Popover Size Options in Radix UI with JSX
DESCRIPTION: This snippet illustrates various implementations of the Popover component, each with different size configurations in Radix UI using JSX. By setting size properties, the Popover.Content adjusts its internal padding and border-radius, demonstrating how to customize size for different use cases. Dependencies include Radix UI components and its style props, ensuring the output popovers match desired dimensions.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/popover.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Flex gap="4" align="center">
	<Popover.Root>
		<Popover.Trigger>
			<Button variant="soft">Size 1</Button>
		</Popover.Trigger>
		<Popover.Content __size__="1" maxWidth="300px">
			<Text as="p" trim="both" size="1">
				The quick brown fox jumps over the lazy dog.
			</Text>
		</Popover.Content>
	</Popover.Root>

	<Popover.Root>
		<Popover.Trigger>
			<Button variant="soft">Size 2</Button>
		</Popover.Trigger>
		<Popover.Content __size__="2" maxWidth="400px">
			<Text as="p" trim="both" size="2">
				The quick brown fox jumps over the lazy dog.
			</Text>
		</Popover.Content>
	</Popover.Root>

	<Popover.Root>
		<Popover.Trigger>
			<Button variant="soft">Size 3</Button>
		</Popover.Trigger>
		<Popover.Content __size__="3" maxWidth="500px">
			<Text as="p" trim="both" size="3">
				The quick brown fox jumps over the lazy dog.
			</Text>
		</Popover.Content>
	</Popover.Root>

	<Popover.Root>
		<Popover.Trigger>
			<Button variant="soft">Size 4</Button>
		</Popover.Trigger>
		<Popover.Content __size__="4">
			<Text as="p" trim="both" size="4">
				The quick brown fox jumps over the lazy dog.
			</Text>
		</Popover.Content>
	</Popover.Root>
</Flex>
```

----------------------------------------

TITLE: Creating Context Menu with Complex Items in React using Radix UI
DESCRIPTION: Example showing how to add decorative elements like images to Context Menu items, creating more complex and visually rich menu options.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/context-menu.mdx#2025-04-21_snippet_17

LANGUAGE: jsx
CODE:
```
import { ContextMenu } from "radix-ui";

export default () => (
	<ContextMenu.Root>
		<ContextMenu.Trigger>…</ContextMenu.Trigger>
		<ContextMenu.Portal>
			<ContextMenu.Content>
				<ContextMenu.Item>
					<img src="…" />
					Adolfo Hess
				</ContextMenu.Item>
				<ContextMenu.Item>
					<img src="…" />
					Miyah Myles
				</ContextMenu.Item>
			</ContextMenu.Content>
		</ContextMenu.Portal>
	</ContextMenu.Root>
);
```

----------------------------------------

TITLE: Adding an Indicator to Radix UI Navigation Menu
DESCRIPTION: This snippet illustrates adding an `Indicator` to the Navigation Menu, providing a visual cue for the active `Trigger`. It includes necessary imports and a basic structure for the indicator within the `NavigationMenu.List` component. The styles for the indicator are defined in a separate CSS snippet.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/navigation-menu.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
// index.jsx
import { NavigationMenu } from "radix-ui";
import "./styles.css";

export default () => (
	<NavigationMenu.Root>
		<NavigationMenu.List>
			<NavigationMenu.Item>
				<NavigationMenu.Trigger>Item one</NavigationMenu.Trigger>
				<NavigationMenu.Content>Item one content</NavigationMenu.Content>
			</NavigationMenu.Item>
			<NavigationMenu.Item>
				<NavigationMenu.Trigger>Item two</NavigationMenu.Trigger>
				<NavigationMenu.Content>Item two content</NavigationMenu.Content>
			</NavigationMenu.Item>

			<NavigationMenu.Indicator __className__="NavigationMenuIndicator" />
		</NavigationMenu.List>

		<NavigationMenu.Viewport />
	</NavigationMenu.Root>
);
```

----------------------------------------

TITLE: Using Multiple Semantic Aliases for the Same Color Scale in JavaScript
DESCRIPTION: This snippet shows how to handle situations where multiple semantic terms need to map to the same color scale in JavaScript. It creates a theme object with multiple semantic aliases (like 'info'/'accent', 'valid'/'success', 'pending'/'warning') that reference the same Radix color values.
SOURCE: https://github.com/radix-ui/website/blob/main/data/colors/docs/overview/aliasing.mdx#2025-04-21_snippet_3

LANGUAGE: javascript
CODE:
```
import {
	blue,
	green,
	yellow
} from "@radix-ui/colors";

const theme = {
	...blue,
	...green,
	...yellow,

	accent1: blue.blue1,
	accent2: blue.blue2,
	info1: blue.blue1,
	info2: blue.blue2,

	success1: green.green1,
	success2: green.green2,
	valid1: green.green1,
	valid2: green.green2,

	warning1: yellow.yellow1,
	warning2: yellow.yellow2,
	pending1: yellow.yellow1,
	pending2: yellow.yellow2,
};
```

----------------------------------------

TITLE: Configuring Checkbox Card Sizes in React
DESCRIPTION: Shows how to set different size variations for checkbox cards using the size prop
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/checkbox-cards.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Flex align="center" gap="3">
	<CheckboxCards.Root defaultValue={["1"]} size="1">
		<CheckboxCards.Item value="1">Agree to Terms</CheckboxCards.Item>
	</CheckboxCards.Root>

	<CheckboxCards.Root defaultValue={["1"]} size="2">
		<CheckboxCards.Item value="1">Agree to Terms</CheckboxCards.Item>
	</CheckboxCards.Root>

	<CheckboxCards.Root defaultValue={["1"]} size="3">
		<CheckboxCards.Item value="1">Agree to Terms</CheckboxCards.Item>
	</CheckboxCards.Root>
</Flex>
```

----------------------------------------

TITLE: Auto-Submitting Form with One-Time Passwords
DESCRIPTION: Shows how to use the autoSubmit prop to automatically submit a form when the One-Time Password Field is completely filled. The example includes a handleSubmit function that verifies the entered code against a validCode and performs navigation on success.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/one-time-password-field.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
function Verify({ validCode }) {
	const PASSWORD_LENGTH = 6;
	function handleSubmit(event) {
		event.preventDefault();
		const formData = event.formData;
		if (formData.get("otp") === validCode) {
			redirect("/authenticated");
		} else {
			window.alert("Invalid code");
		}
	}
	return (
		<form onSubmit={handleSubmit}>
			<OneTimePasswordField.Root name="otp" autoSubmit>
				{PASSWORD_LENGTH.map((_, i) => (
					<OneTimePasswordField.Input key={i} />
				))}
				{/* HiddenInput is required for the form to have data associated with the field */}
				<OneTimePasswordField.HiddenInput />
			</OneTimePasswordField.Root>
			<button>Submit</button>
		</form>
	);
}
```

----------------------------------------

TITLE: Applying Custom Font in CSS
DESCRIPTION: Shows how to assign a custom font loaded with next/font to the default font family in Radix Themes.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/typography.mdx#2025-04-21_snippet_6

LANGUAGE: css
CODE:
```
.radix-themes {
	--default-font-family: var(--font-inter);
}
```

----------------------------------------

TITLE: Positioning Select Menu in React
DESCRIPTION: This snippet demonstrates how to use the 'position' prop to position the select menu below the trigger using the 'popper' value.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/select.mdx#2025-04-21_snippet_8

LANGUAGE: jsx
CODE:
```
<Select.Root defaultValue="apple">
	<Select.Trigger />
	<Select.Content position="popper">
		<Select.Item value="apple">Apple</Select.Item>
		<Select.Item value="orange">Orange</Select.Item>
	</Select.Content>
</Select.Root>
```

----------------------------------------

TITLE: Constraining Content Size with CSS Variables
DESCRIPTION: This CSS snippet constrains the width of the Popover.Content to match the trigger width and limits its maximum height using CSS custom properties exposed by Radix UI. It uses `--radix-popover-trigger-width` and `--radix-popover-content-available-height` to achieve this.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/popover.mdx#2025-04-21_snippet_3

LANGUAGE: css
CODE:
```
"/* styles.css */
.PopoverContent {
	width: var(__--radix-popover-trigger-width__);
	max-height: var(__--radix-popover-content-available-height__);
}"
```

----------------------------------------

TITLE: CSS for Origin-Aware HoverCard Animation
DESCRIPTION: This CSS snippet applies an origin-aware animation to the 'HoverCardContent' class, using the `--radix-hover-card-content-transform-origin` CSS custom property to dynamically set the transform origin based on the HoverCard's position. It also defines a 'scaleIn' animation that scales the content from 0 to 1, providing a smooth appearance.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/hover-card.mdx#2025-04-21_snippet_6

LANGUAGE: css
CODE:
```
/* styles.css */
.HoverCardContent {
	transform-origin: var(__--radix-hover-card-content-transform-origin__);
	animation: scaleIn 0.5s ease-out;
}

@keyframes scaleIn {
	from {
		opacity: 0;
		transform: scale(0);
	}
	to {
		opacity: 1;
		transform: scale(1);
	}
}
```

----------------------------------------

TITLE: Creating Semantic Color Aliases in CSS with Radix Colors
DESCRIPTION: This snippet demonstrates how to import Radix color scales via CDN and create semantic aliases by mapping color scales to purpose-based variable names. It shows mappings for accent, success, warning, and danger colors using CSS custom properties.
SOURCE: https://github.com/radix-ui/website/blob/main/data/colors/docs/overview/aliasing.mdx#2025-04-21_snippet_0

LANGUAGE: css
CODE:
```
/*
 * Note: Importing from the CDN in production is not recommended.
 * It's intended for prototyping only.
 */
@import "https://cdn.jsdelivr.net/npm/@radix-ui/colors@latest/blue.css";
@import "https://cdn.jsdelivr.net/npm/@radix-ui/colors@latest/green.css";
@import "https://cdn.jsdelivr.net/npm/@radix-ui/colors@latest/yellow.css";
@import "https://cdn.jsdelivr.net/npm/@radix-ui/colors@latest/red.css";

:root {
	--accent-1: var(--blue-1);
	--accent-2: var(--blue-2);
	--accent-3: var(--blue-3);
	--accent-4: var(--blue-4);
	--accent-5: var(--blue-5);
	--accent-6: var(--blue-6);
	--accent-7: var(--blue-7);
	--accent-8: var(--blue-8);
	--accent-9: var(--blue-9);
	--accent-10: var(--blue-10);
	--accent-11: var(--blue-11);
	--accent-12: var(--blue-12);

	--success-1: var(--green-1);
	--success-2: var(--green-2);
	/* repeat for all steps */

	--warning-1: var(--yellow-1);
	--warning-2: var(--yellow-2);
	/* repeat for all steps */

	--danger-1: var(--red-1);
	--danger-2: var(--red-2);
	/* repeat for all steps */
}
```

----------------------------------------

TITLE: Implementing Basic Switch Component Structure
DESCRIPTION: Basic implementation showing the anatomy of a Switch component using Radix UI, demonstrating how to compose the Root and Thumb parts together.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/switch.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { Switch } from "radix-ui";

export default () => (
	<Switch.Root>
		<Switch.Thumb />
	</Switch.Root>
);
```

----------------------------------------

TITLE: Controlling Checkbox Size - React
DESCRIPTION: This snippet shows how to use the `size` prop to control the sizes of individual checkbox items in a Checkbox Group. It effectively demonstrates different size configurations through multiple instances of Checkbox Group.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/checkbox-group.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Flex align="center" gap="2">
	<CheckboxGroup.Root size="1" defaultValue="1">
		<CheckboxGroup.Item value="1" />
	</CheckboxGroup.Root>

	<CheckboxGroup.Root size="2" defaultValue="1">
		<CheckboxGroup.Item value="1" />
	</CheckboxGroup.Root>

	<CheckboxGroup.Root size="3" defaultValue="1">
		<CheckboxGroup.Item value="1" />
	</CheckboxGroup.Root>
</Flex>
```

----------------------------------------

TITLE: Duplicate Toasts Implementation in React
DESCRIPTION: This snippet illustrates how to render multiple instances of the same toast by using React state to manage the number of toasts to display.  Each time the save button is clicked, a new toast is rendered.  This demonstrates a declarative approach to toast duplication.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/toast.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
export default () => {
	const [savedCount, setSavedCount] = React.useState(0);

	return (
		<div>
			<form onSubmit={() => setSavedCount((count) => count + 1)}>
				{/* ... */}
				<button>save</button>
			</form>

			{Array.from({ length: savedCount }).map((_, index) => (
				<Toast.Root key={index}>
					<Toast.Description>Saved!</Toast.Description>
				</Toast.Root>
			))}
		</div>
	);
};
```

----------------------------------------

TITLE: Custom Portal Container for Dialogs - JSX
DESCRIPTION: This snippet demonstrates how to customize the portal container for a dialog using Radix UI. It maintains state for the container element and uses the 'Dialog.Portal' to render the dialog into the specified container. Inputs include a reference to the container element, and it outputs a dialog that appears in the custom-defined container.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/dialog.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
// Custom portal container
import * as React from "react";
import { Dialog } from "radix-ui";

export default () => {
	const [container, setContainer] = React.useState(null);
	return (
		<div>
			<Dialog.Root>
				<Dialog.Trigger />
				<Dialog.Portal __container__={container}>
					<Dialog.Overlay />
					<Dialog.Content>...</Dialog.Content>
				</Dialog.Portal>
			</Dialog.Root>

			<div ref={__setContainer__} />
		</div>
	);
};
```

----------------------------------------

TITLE: Constraining Context Menu Size with CSS Custom Properties in Radix UI
DESCRIPTION: Demonstrates how to use Radix UI's CSS custom properties to constrain the width and height of the Context Menu content to match the trigger width and available viewport height.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/context-menu.mdx#2025-04-21_snippet_18

LANGUAGE: jsx
CODE:
```
// index.jsx
import { ContextMenu } from "radix-ui";
import "./styles.css";

export default () => (
	<ContextMenu.Root>
		<ContextMenu.Trigger>…</ContextMenu.Trigger>
		<ContextMenu.Portal>
			<ContextMenu.Content __className__="ContextMenuContent">
				…
			</ContextMenu.Content>
		</ContextMenu.Portal>
	</ContextMenu.Root>
);
```

LANGUAGE: css
CODE:
```
/* styles.css */
.ContextMenuContent {
	width: var(__--radix-context-menu-trigger-width__);
	max-height: var(__--radix-context-menu-content-available-height__);
}
```

----------------------------------------

TITLE: Modifying Callout Size in React
DESCRIPTION: This snippet illustrates how to control the size of callout components using the `size` prop. Different size levels are defined to affect the overall appearance of the callout. This can be particularly useful for adjusting the presentation based on the UI requirements. It requires Radix UI and components like `Flex` for layout management.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/callout.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Flex direction=\"column\" gap=\"3\" align=\"start\">\n\t<Callout.Root size=\"1\">\n\t\t<Callout.Icon>\n\t\t\t<InfoCircledIcon />\n\t\t</Callout.Icon>\n\t\t<Callout.Text>\n\t\t\tYou will need admin privileges to install and access this application.\n\t\t</Callout.Text>\n\t</Callout.Root>\n\n\t<Callout.Root size=\"2\">\n\t\t<Callout.Icon>\n\t\t\t<InfoCircledIcon />\n\t\t</Callout.Icon>\n\t\t<Callout.Text>\n\t\t\tYou will need admin privileges to install and access this application.\n\t\t</Callout.Text>\n\t</Callout.Root>\n\n\t<Callout.Root size=\"3\">\n\t\t<Callout.Icon>\n\t\t\t<InfoCircledIcon />\n\t\t</Callout.Icon>\n\t\t<Callout.Text>\n\t\t\tYou will need admin privileges to install and access this application.\n\t\t</Callout.Text>\n\t</Callout.Root>\n</Flex>
```

----------------------------------------

TITLE: Defining CheckboxItem Properties in React
DESCRIPTION: This snippet defines properties for the CheckboxItem component that determine its behavior and interaction. It includes properties like 'asChild', 'checked', 'onCheckedChange', and 'disabled', along with their types and descriptions. It is intended for use in a React application, leveraging controlled component patterns.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/context-menu.mdx#2025-04-21_snippet_3

LANGUAGE: javascript
CODE:
```
<PropsTable
	data={[
		{
			name: "asChild",
			required: false,
			type: "boolean",
			default: "false",
			description: (
				<>Change the default rendered element for the one passed as a child,
				merging their props and behavior.<br /><br />Read our <a href="../guides/composition">Composition</a> guide for more details.</>
			),
		},
		{
			name: "checked",
			type: `boolean | 'indeterminate'`,
			description: (
				<span>The controlled checked state of the item. Must be used in conjunction with <Code>onCheckedChange</Code>.</span>
			),
		},
		{
			name: "onCheckedChange",
			type: `(checked: boolean) => void`,
			typeSimple: "function",
			description: (
				<span>Event handler called when the checked state changes.</span>
			),
		},
		{
			name: "disabled",
			type: "boolean",
			description: (
				<span>When <Code>true</Code>, prevents the user from interacting with the item.</span>
			),
		},
		{
			name: "onSelect",
			type: "(event: Event) => void",
			typeSimple: "function",
			description: (
				<span>Event handler called when the user selects an item (via mouse or keyboard). Calling <Code>event.preventDefault</Code> in this handler will prevent the context menu from closing when selecting that item.</span>
			),
		},
		{
			name: "textValue",
			type: "string",
			description: (
				<span>Optional text used for typeahead purposes. By default the typeahead behavior will use the <Code>.textContent</Code> of the item. Use this when the content is complex, or you have non-textual content inside.</span>
			),
		},
	]}/>
```

----------------------------------------

TITLE: Basic Skeleton Usage in React
DESCRIPTION: Simple implementation of the Skeleton component wrapping loading text.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/skeleton.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Skeleton>Loading</Skeleton>
```

----------------------------------------

TITLE: Using Labels in DropdownMenu with React
DESCRIPTION: This example shows how to use the Label part to add labels to sections in a DropdownMenu.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/dropdown-menu.mdx#2025-04-21_snippet_10

LANGUAGE: jsx
CODE:
```
<DropdownMenu.Root>
	<DropdownMenu.Trigger>…</DropdownMenu.Trigger>
	<DropdownMenu.Portal>
		<DropdownMenu.Content>
			<DropdownMenu.Label>Label</DropdownMenu.Label>
			<DropdownMenu.Item>…</DropdownMenu.Item>
			<DropdownMenu.Item>…</DropdownMenu.Item>
			<DropdownMenu.Item>…</DropdownMenu.Item>
		</DropdownMenu.Content>
	</DropdownMenu.Portal>
</DropdownMenu.Root>
```

----------------------------------------

TITLE: Configuring Radio Group Size in JSX
DESCRIPTION: Shows how to use the 'size' prop to control the radio button size with three different size options.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/radio-group.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Flex align="center" gap="2">
	<RadioGroup.Root size="1" defaultValue="1">
		<RadioGroup.Item value="1" />
	</RadioGroup.Root>

	<RadioGroup.Root size="2" defaultValue="1">
		<RadioGroup.Item value="1" />
	</RadioGroup.Root>

	<RadioGroup.Root size="3" defaultValue="1">
		<RadioGroup.Item value="1" />
	</RadioGroup.Root>
</Flex>
```

----------------------------------------

TITLE: Creating an Inset Layout with Image and Text
DESCRIPTION: Demonstrates the Inset component usage with a Card, image, and text content. Shows how to apply clipping, positioning, and responsive image styling.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/inset.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Box maxWidth="240px">
	<Card size="2">
		<Inset clip="padding-box" side="top" pb="current">
			<img
				src="https://images.unsplash.com/photo-1617050318658-a9a3175e34cb?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=600&q=80"
				alt="Bold typography"
				style={{
					display: "block",
					objectFit: "cover",
					width: "100%",
					height: 140,
					backgroundColor: "var(--gray-5)",
				}}
			/>
		</Inset>
		<Text as="p" size="3">
			<Strong>Typography</Strong> is the art and technique of arranging type to
			make written language legible, readable and appealing when displayed.
		</Text>
	</Card>
</Box>
```

----------------------------------------

TITLE: Using High-contrast Avatars in a Grid
DESCRIPTION: This snippet demonstrates how to use the 'highContrast' prop to enhance color contrast of Avatar components. The example displays pairs of solid variant Avatars in a Grid layout, one with high contrast and one without.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/avatar.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
<Grid rows="2" gap="2" display="inline-grid" flow="column">
	<Avatar variant="solid" color="indigo" fallback="A" />
	<Avatar variant="solid" color="indigo" fallback="A" highContrast />
	<Avatar variant="solid" color="cyan" fallback="A" />
	<Avatar variant="solid" color="cyan" fallback="A" highContrast />
	<Avatar variant="solid" color="orange" fallback="A" />
	<Avatar variant="solid" color="orange" fallback="A" highContrast />
	<Avatar variant="solid" color="crimson" fallback="A" />
	<Avatar variant="solid" color="crimson" fallback="A" highContrast />
	<Avatar variant="solid" color="gray" fallback="A" />
	<Avatar variant="solid" color="gray" fallback="A" highContrast />
</Grid>
```

----------------------------------------

TITLE: Segmented Controls in One-Time Password Fields
DESCRIPTION: Demonstrates how to create a visually segmented One-Time Password Field using separators between inputs. The Root component can accept arbitrary children, such as separators, made accessible with aria-hidden to assistive technologies, and maintaining the group role.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/one-time-password-field.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<OneTimePasswordField.Root>
	<OneTimePasswordField.Input />
	<Separator.Root aria-hidden />
	<OneTimePasswordField.Input />
	<Separator.Root aria-hidden />
	<OneTimePasswordField.Input />
	<Separator.Root aria-hidden />
	<OneTimePasswordField.Input />
	<OneTimePasswordField.HiddenInput />
</OneTimePasswordField.Root>
```

----------------------------------------

TITLE: Constraining Select Content Dimensions with CSS Variables
DESCRIPTION: Illustrates how to use Radix UI's CSS custom properties to control Select content width and height based on trigger dimensions
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/select.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
import { Select } from "radix-ui";
import "./styles.css";

export default () => (
	<Select.Root>
		<Select.Trigger>…</Select.Trigger>
		<Select.Portal>
			<Select.Content
				__className__="SelectContent"
				position="popper"
				sideOffset={5}
			>
				…
			</Select.Content>
		</Select.Portal>
	</Select.Root>
);
```

LANGUAGE: css
CODE:
```
.SelectContent {
	width: var(__--radix-select-trigger-width__);
	max-height: var(__--radix-select-content-available-height__);
}
```

----------------------------------------

TITLE: Aligning Checkbox Items with Text - React
DESCRIPTION: This snippet shows how to compose Checkbox Group items within Text elements, ensuring proper alignment with the text. It's demonstrated with various sizes to show consistent alignment behavior.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/checkbox-group.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="3">
	<CheckboxGroup.Root size="1" defaultValue="1">
		<Text as="label" size="2">
			<Flex gap="2">
				<CheckboxGroup.Item value="1" /> Default
			</Flex>
		</Text>

		<Text as="label" size="2">
			<Flex gap="2">
				<CheckboxGroup.Item value="2" /> Compact
			</Flex>
		</Text>
	</CheckboxGroup.Root>

	<CheckboxGroup.Root size="2" defaultValue="1">
		<Text as="label" size="3">
			<Flex gap="2">
				<CheckboxGroup.Item value="1" /> Default
			</Flex>
		</Text>

		<Text as="label" size="3">
			<Flex gap="2">
				<CheckboxGroup.Item value="2" /> Compact
			</Flex>
		</Text>
	</CheckboxGroup.Root>

	<CheckboxGroup.Root size="3" defaultValue="1">
		<Text as="label" size="4">
			<Flex gap="2">
				<CheckboxGroup.Item value="1" /> Default
			</Flex>
		</Text>

		<Text as="label" size="4">
			<Flex gap="2">
				<CheckboxGroup.Item value="2" /> Compact
			</Flex>
		</Text>
	</CheckboxGroup.Root>
</Flex>
```

----------------------------------------

TITLE: Implementing Complex Items in Radix UI Menubar (JSX)
DESCRIPTION: This snippet demonstrates how to add complex items with images to a Radix UI Menubar component. It shows the structure of the Menubar with nested components and how to include decorative elements like images within menu items.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/menubar.mdx#2025-04-21_snippet_12

LANGUAGE: jsx
CODE:
```
import { Menubar } from "radix-ui";

export default () => (
	<Menubar.Root>
		<Menubar.Menu>
			<Menubar.Trigger>…</Menubar.Trigger>
			<Menubar.Portal>
				<Menubar.Content>
					<Menubar.Item>
						<img src="…" />
						Adolfo Hess
					</Menubar.Item>
					<Menubar.Item>
						<img src="…" />
						Miyah Myles
					</Menubar.Item>
				</Menubar.Content>
			</Menubar.Portal>
		</Menubar.Menu>
	</Menubar.Root>
);
```

----------------------------------------

TITLE: Setting Theme Accent Color in React
DESCRIPTION: Shows how to set the accent color on the Theme component
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/color.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Theme accentColor="indigo">
	<MyApp />
</Theme>
```

----------------------------------------

TITLE: Setting Heading Color
DESCRIPTION: This snippet illustrates how to specify custom colors for the Heading text using the 'color' prop, ensuring contrast with the background.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/heading.mdx#2025-04-21_snippet_8

LANGUAGE: jsx
CODE:
```
<Flex direction="column">
	<Heading color="indigo">The quick brown fox jumps over the lazy dog</Heading>
	<Heading color="cyan">The quick brown fox jumps over the lazy dog</Heading>
	<Heading color="orange">The quick brown fox jumps over the lazy dog</Heading>
	<Heading color="crimson">The quick brown fox jumps over the lazy dog</Heading>
</Flex>
```

----------------------------------------

TITLE: Setting Avatar Radius in a Flex Container
DESCRIPTION: This snippet shows how to use the 'radius' prop to set the border radius of Avatar components. It displays several Avatar components with different radius values within a Flex container.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/avatar.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
<Flex gap="2">
	<Avatar radius="none" fallback="A" />
	<Avatar radius="large" fallback="A" />
	<Avatar radius="full" fallback="A" />
</Flex>
```

----------------------------------------

TITLE: Adjusting Badge Radius in JSX
DESCRIPTION: Using the 'radius' prop, this code snippet alters the border-radius of Badge components to none, large, or full. This demonstrates flexible UI customization within Radix UI's Flex component setup.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/badge.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
<Flex gap="2">\n	<Badge variant="solid" radius="none" color="indigo">\n		New\n	</Badge>\n	<Badge variant="solid" radius="large" color="indigo">\n		New\n	</Badge>\n	<Badge variant="solid" radius="full" color="indigo">\n		New\n	</Badge>\n</Flex>
```

----------------------------------------

TITLE: Color Customization of Avatars in a Flex Container
DESCRIPTION: This snippet illustrates the use of the 'color' prop to assign specific colors to Avatar components. It showcases several Avatar components of solid variant with different colors within a Flex container.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/avatar.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Flex gap="2">
	<Avatar variant="solid" color="indigo" fallback="A" />
	<Avatar variant="solid" color="cyan" fallback="A" />
	<Avatar variant="solid" color="orange" fallback="A" />
	<Avatar variant="solid" color="crimson" fallback="A" />
</Flex>
```

----------------------------------------

TITLE: High Contrast Button Example
DESCRIPTION: Demonstrates the use of highContrast prop on buttons
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/color.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
<Flex gap="4">
	<Button variant="classic" color="gray">
		Edit profile
	</Button>
	<Button variant="classic" color="gray" highContrast>
		Edit profile
	</Button>
</Flex>
```

----------------------------------------

TITLE: High-Contrast Checkbox Cards in React
DESCRIPTION: Demonstrates the use of high-contrast mode for improved visual accessibility across different color themes
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/checkbox-cards.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
<Grid columns="2" gap="3" display="inline-grid">
	<CheckboxCards.Root defaultValue={["1"]} color="indigo">
		<CheckboxCards.Item value="1">Agree to Terms</CheckboxCards.Item>
	</CheckboxCards.Root>

	<CheckboxCards.Root defaultValue={["1"]} color="indigo" highContrast>
		<CheckboxCards.Item value="1">Agree to Terms</CheckboxCards.Item>
	</CheckboxCards.Root>

	<CheckboxCards.Root defaultValue={["1"]} color="cyan">
		<CheckboxCards.Item value="1">Agree to Terms</CheckboxCards.Item>
	</CheckboxCards.Root>

	<CheckboxCards.Root defaultValue={["1"]} color="cyan" highContrast>
		<CheckboxCards.Item value="1">Agree to Terms</CheckboxCards.Item>
	</CheckboxCards.Root>

	<CheckboxCards.Root defaultValue={["1"]} color="orange">
		<CheckboxCards.Item value="1">Agree to Terms</CheckboxCards.Item>
	</CheckboxCards.Root>

	<CheckboxCards.Root defaultValue={["1"]} color="orange" highContrast>
		<CheckboxCards.Item value="1">Agree to Terms</CheckboxCards.Item>
	</CheckboxCards.Root>

	<CheckboxCards.Root defaultValue={["1"]} color="crimson">
		<CheckboxCards.Item value="1">Agree to Terms</CheckboxCards.Item>
	</CheckboxCards.Root>

	<CheckboxCards.Root defaultValue={["1"]} color="crimson" highContrast>
		<CheckboxCards.Item value="1">Agree to Terms</CheckboxCards.Item>
	</CheckboxCards.Root>
</Grid>
```

----------------------------------------

TITLE: Importing and Using Label Component in React
DESCRIPTION: Example of how to import and implement the Label component in a React application. Shows the basic structure for using the Root component.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/label.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
import { Label } from "radix-ui";

export default () => <Label.Root />;
```

----------------------------------------

TITLE: Initializing Checkbox Cards with Multiple Items in React
DESCRIPTION: Demonstrates creating a checkbox card group with multiple selectable items, using default value and responsive column layout
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/checkbox-cards.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Box maxWidth="600px">
	<CheckboxCards.Root defaultValue={["1"]} columns={{ initial: "1", sm: "3" }}>
		<CheckboxCards.Item value="1">
			<Flex direction="column" width="100%">
				<Text weight="bold">A1 Keyboard</Text>
				<Text>US Layout</Text>
			</Flex>
		</CheckboxCards.Item>
		<CheckboxCards.Item value="2">
			<Flex direction="column" width="100%">
				<Text weight="bold">Pro Mouse</Text>
				<Text>Zero-lag wireless</Text>
			</Flex>
		</CheckboxCards.Item>
		<CheckboxCards.Item value="3">
			<Flex direction="column" width="100%">
				<Text weight="bold">Lightning Mat</Text>
				<Text>Wireless charging</Text>
			</Flex>
		</CheckboxCards.Item>
	</CheckboxCards.Root>
</Box>
```

----------------------------------------

TITLE: Text Component with Truncation in JSX
DESCRIPTION: Example showing how to use the 'truncate' prop to truncate text with an ellipsis when it overflows its container, useful for preventing unwanted line wrapping.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/text.mdx#2025-04-21_snippet_10

LANGUAGE: jsx
CODE:
```
<Flex maxWidth="300px">
	<Text truncate>
		The goal of typography is to relate font size, line height, and line width
		in a proportional way that maximizes beauty and makes reading easier and
		more pleasant.
	</Text>
</Flex>
```

----------------------------------------

TITLE: Using Weight Prop in Blockquote Component with JSX
DESCRIPTION: This snippet exemplifies how to use the 'weight' prop to set the font weight of the text within a Blockquote. It contains variations for 'regular', 'medium', and 'bold' weights demonstrated in a Flex layout.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/blockquote.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="3" maxWidth="500px">
	<Blockquote weight="regular">
		Perfect typography is certainly the most elusive of all arts. Sculpture in
		stone alone comes near it in obstinacy.
	</Blockquote>

	<Blockquote weight="medium">
		Perfect typography is certainly the most elusive of all arts. Sculpture in
		stone alone comes near it in obstinacy.
	</Blockquote>

	<Blockquote weight="bold">
		Perfect typography is certainly the most elusive of all arts. Sculpture in
		stone alone comes near it in obstinacy.
	</Blockquote>
</Flex>
```

----------------------------------------

TITLE: Styling Disabled Items in ContextMenu with React
DESCRIPTION: Example showing how to implement and style disabled items in a ContextMenu using the data-disabled attribute and CSS.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/context-menu.mdx#2025-04-21_snippet_12

LANGUAGE: jsx
CODE:
```
// index.jsx
import { ContextMenu } from "radix-ui";
import "./styles.css";

export default () => (
	<ContextMenu.Root>
		<ContextMenu.Trigger>…</ContextMenu.Trigger>
		<ContextMenu.Portal>
			<ContextMenu.Content>
				<ContextMenu.Item __className__="ContextMenuItem" __disabled__>
					…
				</ContextMenu.Item>
				<ContextMenu.Item className="ContextMenuItem">…</ContextMenu.Item>
			</ContextMenu.Content>
		</ContextMenu.Portal>
	</ContextMenu.Root>
);
```

LANGUAGE: css
CODE:
```
/* styles.css */
.ContextMenuItem[__data-disabled__] {
	color: gainsboro;
}
```

----------------------------------------

TITLE: TextArea Color Customization
DESCRIPTION: Examples of TextArea with different color variations using the color prop.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/text-area.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="3" maxWidth="250px">
	<TextArea color="blue" variant="soft" placeholder="Reply to comment…" />
	<TextArea color="green" variant="soft" placeholder="Reply to comment…" />
	<TextArea color="red" variant="soft" placeholder="Reply to comment…" />
</Flex>
```

----------------------------------------

TITLE: Color Styling for Icon Button in JSX
DESCRIPTION: This snippet shows how to use the `color` prop on the `IconButton` to apply a specific color from the theme. Different color values such as "crimson", "indigo", "grass", and "orange" are applied to `IconButton` components, with a `soft` variant.  These colors are likely defined within the Radix UI theme.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/icon-button.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
"<Flex gap=\"3\">\n\t<IconButton color=\"crimson\" variant=\"soft\">\n\t\t<MagnifyingGlassIcon width=\"18\" height=\"18\" />\n\t</IconButton>\n\t<IconButton color=\"indigo\" variant=\"soft\">\n\t\t<MagnifyingGlassIcon width=\"18\" height=\"18\" />\n\t</IconButton>\n\t<IconButton color=\"grass\" variant=\"soft\">\n\t\t<MagnifyingGlassIcon width=\"18\" height=\"18\" />\n\t</IconButton>\n\t<IconButton color=\"orange\" variant=\"soft\">\n\t\t<MagnifyingGlassIcon width=\"18\" height=\"18\" />\n\t</IconButton>\n</Flex>"
```

----------------------------------------

TITLE: Applying Colors to Radio Buttons in JSX
DESCRIPTION: This example shows how to use the 'color' prop to assign specific colors to radio buttons. It displays four radio buttons with different colors: indigo, cyan, orange, and crimson.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/radio.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Flex as="span" gap="2">
	<Radio color="indigo" defaultChecked />
	<Radio color="cyan" defaultChecked />
	<Radio color="orange" defaultChecked />
	<Radio color="crimson" defaultChecked />
</Flex>
```

----------------------------------------

TITLE: Implementing Select with Placeholder
DESCRIPTION: Demonstrates how to add a placeholder to Select component and style it using data-placeholder attribute
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/select.mdx#2025-04-21_snippet_7

LANGUAGE: jsx
CODE:
```
import { Select } from "radix-ui";
import "./styles.css";

export default () => (
	<Select.Root>
		<Select.Trigger __className__="SelectTrigger">
			<Select.Value __placeholder__="Pick an option" />
			<Select.Icon />
		</Select.Trigger>
		<Select.Portal>
			<Select.Content>…</Select.Content>
		</Select.Portal>
	</Select.Root>
);
```

LANGUAGE: css
CODE:
```
.SelectTrigger[__data-placeholder__] {
	color: "gainsboro";
}
```

----------------------------------------

TITLE: Importing Radix UI Components - TypeScript
DESCRIPTION: This snippet shows how to import necessary Radix UI components after installing the package. The imported components can be used within a TypeScript project to build accessible design systems.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/overview/introduction.mdx#2025-04-21_snippet_1

LANGUAGE: typescript
CODE:
```
import { Dialog, DropdownMenu, Tooltip } from "radix-ui";
```

----------------------------------------

TITLE: Increasing Color Contrast for Code Snippets in JSX
DESCRIPTION: This snippet utilizes the 'highContrast' prop to enhance color contrast for the 'Code' component against its background. It provides examples with and without high contrast.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/code.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
<Flex gap="3">
	<Flex direction="column" align="start" gap="2">
		<Code color="gray" variant="solid">
			console.log()
		</Code>
		<Code color="gray" variant="soft">
			console.log()
		</Code>
		<Code color="gray" variant="outline">
			console.log()
		</Code>
		<Code color="gray" variant="ghost">
			console.log()
		</Code>
	</Flex>

	<Flex direction="column" align="start" gap="2">
		<Code color="gray" variant="solid" highContrast>
			console.log()
		</Code>
		<Code color="gray" variant="soft" highContrast>
			console.log()
		</Code>
		<Code color="gray" variant="outline" highContrast>
			console.log()
		</Code>
		<Code color="gray" variant="ghost" highContrast>
			console.log()
		</Code>
	</Flex>
</Flex>
```

----------------------------------------

TITLE: Styling for Popover Components using CSS
DESCRIPTION: This CSS snippet defines the styles for the Popover components, targeting the Popover.Trigger, Popover.Content, and Popover.Arrow, ensuring adequate spacing and background properties for better presentation.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/overview/getting-started.mdx#2025-04-21_snippet_3

LANGUAGE: css
CODE:
```
/* styles.css */
.PopoverTrigger {
	background-color: white;
	border-radius: 4px;
}

.PopoverContent {
	border-radius: 4px;
	padding: 20px;
	width: 260px;
	background-color: white;
}

.PopoverArrow {
	fill: white;
}
```

----------------------------------------

TITLE: Truncating Overflowing Code Snippets in JSX
DESCRIPTION: This snippet shows how to use the 'truncate' prop to truncate text with an ellipsis when it overflows its container, specifically demonstrating use with a long CSS gradient.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/code.mdx#2025-04-21_snippet_6

LANGUAGE: jsx
CODE:
```
<Flex maxWidth="200px">
	<Code truncate>linear-gradient(red, orange, yellow, green, blue);</Code>
</Flex>
```

----------------------------------------

TITLE: High Contrast Icon Button in JSX
DESCRIPTION: This snippet demonstrates the use of the `highContrast` prop to increase the color contrast of the `IconButton`. It shows a comparison between IconButtons with and without `highContrast` for different variants and a gray color. This helps in improving accessibility.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/icon-button.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
"<Flex direction=\"column\" gap=\"3\">\n\t<Flex gap=\"3\">\n\t\t<IconButton color=\"gray\" variant=\"classic\">\n\t\t\t<MagnifyingGlassIcon width=\"18\" height=\"18\" />\n\t\t</IconButton>\n\t\t<IconButton color=\"gray\" variant=\"solid\">\n\t\t\t<MagnifyingGlassIcon width=\"18\" height=\"18\" />\n\t\t</IconButton>\n\t\t<IconButton color=\"gray\" variant=\"soft\">\n\t\t\t<MagnifyingGlassIcon width=\"18\" height=\"18\" />\n\t\t</IconButton>\n\t\t<IconButton color=\"gray\" variant=\"surface\">\n\t\t\t<MagnifyingGlassIcon width=\"18\" height=\"18\" />\n\t\t</IconButton>\n\t\t<IconButton color=\"gray\" variant=\"outline\">\n\t\t\t<MagnifyingGlassIcon width=\"18\" height=\"18\" />\n\t\t</IconButton>\n\t</Flex>\n\t<Flex gap=\"3\">\n\t\t<IconButton color=\"gray\" variant=\"classic\" highContrast>\n\t\t\t<MagnifyingGlassIcon width=\"18\" height=\"18\" />\n\t\t</IconButton>\n\t\t<IconButton color=\"gray\" variant=\"solid\" highContrast>\n\t\t\t<MagnifyingGlassIcon width=\"18\" height=\"18\" />\n\t\t</IconButton>\n\t\t<IconButton color=\"gray\" variant=\"soft\" highContrast>\n\t\t\t<MagnifyingGlassIcon width=\"18\" height=\"18\" />\n\t\t</IconButton>\n\t\t<IconButton color=\"gray\" variant=\"surface\" highContrast>\n\t\t\t<MagnifyingGlassIcon width=\"18\" height=\"18\" />\n\t\t</IconButton>\n\t\t<IconButton color=\"gray\" variant=\"outline\" highContrast>\n\t\t\t<MagnifyingGlassIcon width=\"18\" height=\"18\" />\n\t\t</IconButton>\n\t</Flex>\n</Flex>"
```

----------------------------------------

TITLE: Implementing Tooltip with Radix-UI in JavaScript
DESCRIPTION: This snippet initializes a Tooltip component using Radix-UI's Tooltip API, allowing for collision-aware animations. Dependencies include Radix-UI for the Tooltip components and styles.css for animation styles. Expected inputs are the Tooltip.Trigger and Tooltip.Content components that facilitate collision detection.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/tooltip.mdx#2025-04-21_snippet_6

LANGUAGE: jsx
CODE:
```
// index.jsx
import { Tooltip } from "radix-ui";
import "./styles.css";

export default () => (
	<Tooltip.Root>
		<Tooltip.Trigger>…</Tooltip.Trigger>
		<Tooltip.Content __className__="TooltipContent">…</Tooltip.Content>
	</Tooltip.Root>
);
```

----------------------------------------

TITLE: Implementing Section Component with DecorativeBox in JSX
DESCRIPTION: Example showing how to use the Section component wrapped in a DecorativeBox with custom styling. The component is rendered inside a Box with padding and custom background color using CSS variables.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/section.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Box
	py="8"
	style={{ backgroundColor: "var(--gray-a2)", borderRadius: "var(--radius-3)" }}
>
	<DecorativeBox asChild>
		<Section size="2" />
	</DecorativeBox>
</Box>
```

----------------------------------------

TITLE: Implementing High-Contrast Radio Buttons in JSX
DESCRIPTION: This snippet demonstrates the use of the 'highContrast' prop to increase color contrast with the background. It shows a grid of radio buttons with and without high contrast for different colors.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/radio.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
<Grid columns="5" display="inline-grid" gap="2">
	<Radio color="indigo" defaultChecked />
	<Radio color="cyan" defaultChecked />
	<Radio color="orange" defaultChecked />
	<Radio color="crimson" defaultChecked />
	<Radio color="gray" defaultChecked />

	<Radio color="indigo" defaultChecked highContrast />
	<Radio color="cyan" defaultChecked highContrast />
	<Radio color="orange" defaultChecked highContrast />
	<Radio color="crimson" defaultChecked highContrast />
	<Radio color="gray" defaultChecked highContrast />
</Grid>
```

----------------------------------------

TITLE: Using Placeholder in Select Component with React
DESCRIPTION: This example shows how to use the 'placeholder' prop to create a Select Trigger that doesn't need an initial value. It includes grouped options and a separator.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/select.mdx#2025-04-21_snippet_7

LANGUAGE: jsx
CODE:
```
<Select.Root>
	<Select.Trigger placeholder="Pick a fruit" />
	<Select.Content>
		<Select.Group>
			<Select.Label>Fruits</Select.Label>
			<Select.Item value="orange">Orange</Select.Item>
			<Select.Item value="apple">Apple</Select.Item>
			<Select.Item value="grape" disabled>
				Grape
			</Select.Item>
		</Select.Group>
		<Select.Separator />
		<Select.Group>
			<Select.Label>Vegetables</Select.Label>
			<Select.Item value="carrot">Carrot</Select.Item>
			<Select.Item value="potato">Potato</Select.Item>
		</Select.Group>
	</Select.Content>
</Select.Root>
```

----------------------------------------

TITLE: High-contrast Slider Examples in React
DESCRIPTION: Demonstrates the 'highContrast' prop which increases color contrast in light mode. Shows comparison between regular and high contrast variants of different colors.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/slider.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
<Grid columns="2" gap="4">
	<Slider defaultValue={[10]} color="indigo" />
	<Slider defaultValue={[10]} color="indigo" highContrast />
	<Slider defaultValue={[30]} color="cyan" />
	<Slider defaultValue={[30]} color="cyan" highContrast />
	<Slider defaultValue={[50]} color="orange" />
	<Slider defaultValue={[50]} color="orange" highContrast />
	<Slider defaultValue={[70]} color="crimson" />
	<Slider defaultValue={[70]} color="crimson" highContrast />
	<Slider defaultValue={[90]} color="gray" />
	<Slider defaultValue={[90]} color="gray" highContrast />
</Grid>
```

----------------------------------------

TITLE: Increasing Color Contrast
DESCRIPTION: This snippet shows the 'highContrast' prop usage in the Heading component to enhance text visibility against the background.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/heading.mdx#2025-04-21_snippet_9

LANGUAGE: jsx
CODE:
```
<Flex direction="column">
	<Heading color="gray">The quick brown fox jumps over the lazy dog.</Heading>
	<Heading color="gray" highContrast>
		The quick brown fox jumps over the lazy dog.
	</Heading>
</Flex>
```

----------------------------------------

TITLE: Range Slider Implementation in React
DESCRIPTION: Creates a range slider by providing multiple values in the defaultValue array, allowing selection of a range between 25 and 75.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/slider.mdx#2025-04-21_snippet_6

LANGUAGE: jsx
CODE:
```
<Slider defaultValue={[25, 75]} />
```

----------------------------------------

TITLE: Using Abstracted Toast Component in React
DESCRIPTION: This snippet demonstrates how to use the custom Toast component created by abstracting the primitive parts from Radix UI.  It shows a basic usage of the component with a title, content, and children for an action.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/toast.mdx#2025-04-21_snippet_11

LANGUAGE: jsx
CODE:
```
import { Toast } from "./your-toast";

export default () => (
	<Toast title="Upgrade available" content="We've just released Radix 3.0!">
		<button onClick={handleUpgrade}>Upgrade</button>
	</Toast>
);
```

----------------------------------------

TITLE: Setting Default Expanded Item in Accordion
DESCRIPTION: Example of using the defaultValue prop to set an item expanded by default.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/accordion.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Accordion.Root type="single" __defaultValue__="item-2">
	<Accordion.Item value="item-1">…</Accordion.Item>
	<Accordion.Item value="item-2">…</Accordion.Item>
</Accordion.Root>
```

----------------------------------------

TITLE: Implementing High-contrast Radio Groups in JSX
DESCRIPTION: Shows how to use the 'highContrast' prop to increase color contrast with the background for various color options.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/radio-group.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
<Grid rows="2" gap="2" display="inline-grid" flow="column">
	<RadioGroup.Root color="indigo" defaultValue="1">
		<RadioGroup.Item value="1" />
	</RadioGroup.Root>

	<RadioGroup.Root color="indigo" defaultValue="1" highContrast>
		<RadioGroup.Item value="1" />
	</RadioGroup.Root>

	<RadioGroup.Root color="cyan" defaultValue="1">
		<RadioGroup.Item value="1" />
	</RadioGroup.Root>

	<RadioGroup.Root color="cyan" defaultValue="1" highContrast>
		<RadioGroup.Item value="1" />
	</RadioGroup.Root>

	<RadioGroup.Root color="orange" defaultValue="1">
		<RadioGroup.Item value="1" />
	</RadioGroup.Root>

	<RadioGroup.Root color="orange" defaultValue="1" highContrast>
		<RadioGroup.Item value="1" />
	</RadioGroup.Root>

	<RadioGroup.Root color="crimson" defaultValue="1">
		<RadioGroup.Item value="1" />
	</RadioGroup.Root>

	<RadioGroup.Root color="crimson" defaultValue="1" highContrast>
		<RadioGroup.Item value="1" />
	</RadioGroup.Root>

	<RadioGroup.Root color="gray" defaultValue="1">
		<RadioGroup.Item value="1" />
	</RadioGroup.Root>

	<RadioGroup.Root color="gray" defaultValue="1" highContrast>
		<RadioGroup.Item value="1" />
	</RadioGroup.Root>
</Grid>
```

----------------------------------------

TITLE: Event Handler Merging with Slot
DESCRIPTION: Demonstrates how Slot merges event handlers with child precedence and handling of event prevention
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/utilities/slot.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
import { Slot } from "radix-ui";

export default () => (
	<Slot.Root
		onClick={(event) => {
			if (!event.defaultPrevented)
				console.log("Not logged because default is prevented.");
		}}
	>
		<button onClick={(event) => event.preventDefault()} />
	</Slot.Root>
);
```

----------------------------------------

TITLE: Content Size Animation with Keyframes in CSS
DESCRIPTION: This CSS snippet defines the animation rules for the accordion content, specifying the changes in height during open and close animations. It includes keyframes for slide down and slide up effects, utilizing custom properties for the content dimensions. The output is an accordion that smoothly expands and collapses.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/accordion.mdx#2025-04-21_snippet_9

LANGUAGE: css
CODE:
```
/* styles.css */
.AccordionContent {
	overflow: hidden;
}
.AccordionContent[data-state="open"] {
	animation: slideDown 300ms ease-out;
}
.AccordionContent[data-state="closed"] {
	animation: slideUp 300ms ease-out;
}

@keyframes slideDown {
	from {
		height: 0;
	}
	to {
		height: var(__--radix-accordion-content-height__);
	}
}

@keyframes slideUp {
	from {
		height: var(__--radix-accordion-content-height__);
	}
	to {
		height: 0;
	}
}
```

----------------------------------------

TITLE: Setting Text Weight for Code Snippets in JSX
DESCRIPTION: This snippet demonstrates how to use the 'weight' prop to change the text weight of the 'Code' component, illustrating options for regular and bold weights.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/code.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="2" align="start">
	<Code weight="regular">console.log()</Code>
	<Code weight="bold">console.log()</Code>
</Flex>
```

----------------------------------------

TITLE: High-Contrast Link Components
DESCRIPTION: Demonstrates the usage of the `highContrast` prop to increase the color contrast of the Radix UI Link component with its background.  It shows the same link with and without the `highContrast` prop to highlight the difference.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/link.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
<Flex direction="column">
	<Link href="#" color="gray">
		Sign up
	</Link>
	<Link href="#" color="gray" highContrast>
		Sign up
	</Link>
</Flex>
```

----------------------------------------

TITLE: Implementing Context Menu with Radio Items in React using Radix UI
DESCRIPTION: Example of using RadioGroup and RadioItem parts in a Context Menu to create selectable radio options. Uses React state to track the selected value and updates it through the onValueChange handler.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/context-menu.mdx#2025-04-21_snippet_16

LANGUAGE: jsx
CODE:
```
import * as React from "react";
import { CheckIcon } from "@radix-ui/react-icons";
import { ContextMenu } from "radix-ui";

export default () => {
	const [color, setColor] = React.useState("blue");

	return (
		<ContextMenu.Root>
			<ContextMenu.Trigger>…</ContextMenu.Trigger>
			<ContextMenu.Portal>
				<ContextMenu.Content>
					<ContextMenu.RadioGroup value={color} onValueChange={setColor}>
						<ContextMenu.RadioItem value="red">
							<ContextMenu.ItemIndicator>
								<CheckIcon />
							</ContextMenu.ItemIndicator>
							Red
						</ContextMenu.RadioItem>
						<ContextMenu.RadioItem value="blue">
							<ContextMenu.ItemIndicator>
								<CheckIcon />
							</ContextMenu.ItemIndicator>
							Blue
						</ContextMenu.RadioItem>
						<ContextMenu.RadioItem value="green">
							<ContextMenu.ItemIndicator>
								<CheckIcon />
							</ContextMenu.ItemIndicator>
							Green
						</ContextMenu.RadioItem>
					</ContextMenu.RadioGroup>
				</ContextMenu.Content>
			</ContextMenu.Portal>
		</ContextMenu.Root>
	);
};
```

----------------------------------------

TITLE: Rendering Separator Component with Text in JSX
DESCRIPTION: This snippet demonstrates how to use the Separator component within a Text component, including both horizontal and vertical separators in a flex layout.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/separator.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Text size="2">
	Tools for building high-quality, accessible UI.
	<Separator my="3" size="4" />
	<Flex gap="3" align="center">
		Themes
		<Separator orientation="vertical" />
		Primitives
		<Separator orientation="vertical" />
		Icons
		<Separator orientation="vertical" />
		Colors
	</Flex>
</Text>
```

----------------------------------------

TITLE: Select Item Component Props Configuration
DESCRIPTION: Props definition for Select Item with value, disabled state, and typeahead text configuration
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/select.mdx#2025-04-21_snippet_3

LANGUAGE: typescript
CODE:
```
{
  value: string,
  disabled?: boolean,
  textValue?: string,
  asChild?: boolean
}
```

----------------------------------------

TITLE: Alert Dialog with Inset Content
DESCRIPTION: This snippet showcases an Alert Dialog containing inset content with a table. It utilizes the Inset component to align the table content flush with the sides of the dialog, providing a structured layout for displaying additional information within the dialog.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/alert-dialog.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<AlertDialog.Root>
	<AlertDialog.Trigger>
		<Button color="red">Delete users</Button>
	</AlertDialog.Trigger>
	<AlertDialog.Content maxWidth="500px">
		<AlertDialog.Title>Delete Users</AlertDialog.Title>
		<AlertDialog.Description size="2">
			Are you sure you want to delete these users? This action is permanent and
			cannot be undone.
		</AlertDialog.Description>

		<Inset side="x" my="5">
			<Table.Root>
				<Table.Header>
					<Table.Row>
						<Table.ColumnHeaderCell>Full name</Table.ColumnHeaderCell>
						<Table.ColumnHeaderCell>Email</Table.ColumnHeaderCell>
						<Table.ColumnHeaderCell>Group</Table.ColumnHeaderCell>
					</Table.Row>
				</Table.Header>

				<Table.Body>
					<Table.Row>
						<Table.RowHeaderCell>Danilo Sousa</Table.RowHeaderCell>
						<Table.Cell>danilo@example.com</Table.Cell>
						<Table.Cell>Developer</Table.Cell>
					</Table.Row>

					<Table.Row>
						<Table.RowHeaderCell>Zahra Ambessa</Table.RowHeaderCell>
						<Table.Cell>zahra@example.com</Table.Cell>
						<Table.Cell>Admin</Table.Cell>
					</Table.Row>
				</Table.Body>
				</Table.Root>
			</Inset>

			<Flex gap="3" justify="end">
				<AlertDialog.Cancel>
					<Button variant="soft" color="gray">
						Cancel
					</Button>
				</AlertDialog.Cancel>
				<AlertDialog.Action>
					<Button color="red">Delete users</Button>
				</AlertDialog.Action>
			</Flex>
	</AlertDialog.Content>
</AlertDialog.Root>
```

----------------------------------------

TITLE: Sizing Icon Button in JSX
DESCRIPTION: This snippet demonstrates the usage of the `size` prop to control the dimensions of the `IconButton`. The sizes 1, 2, and 3 are used to create buttons of different sizes. The `variant` prop is also used, set to "soft" in this example.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/icon-button.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
"<Flex align=\"center\" gap=\"3\">\n\t<IconButton size=\"1\" variant=\"soft\">\n\t\t<MagnifyingGlassIcon width=\"15\" height=\"15\" />\n\t</IconButton>\n\n\t<IconButton size=\"2\" variant=\"soft\">\n\t\t<MagnifyingGlassIcon width=\"18\" height=\"18\" />\n\t</IconButton>\n\n\t<IconButton size=\"3\" variant=\"soft\">\n\t\t<MagnifyingGlassIcon width=\"22\" height=\"22\" />\n\t</IconButton>\n</Flex>"
```

----------------------------------------

TITLE: Truncated Link Component
DESCRIPTION: Demonstrates the `truncate` prop to truncate the text within a Radix UI Link component with an ellipsis when it exceeds the container's width. A `maxWidth` is applied to the Flex container to constrain the link's width.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/link.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Flex maxWidth="150px">
	<Link href="#" truncate>
		Sign up to the newsletter
	</Link>
</Flex>
```

----------------------------------------

TITLE: Importing and Using Direction.Provider in React
DESCRIPTION: This snippet shows how to import the Direction.Provider component from the Radix UI library and use it to wrap a React application. By utilizing this component, developers can ensure that all child components inherit the specified reading direction.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/utilities/direction-provider.mdx#2025-04-21_snippet_1

LANGUAGE: JavaScript
CODE:
```
import { Direction } from "radix-ui";

export default () => <Direction.Provider />;
```

----------------------------------------

TITLE: Customizing Separator Colors in JSX
DESCRIPTION: This example demonstrates how to use the 'color' prop to assign specific colors to separators, using predefined color values.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/separator.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="3">
	<Separator color="indigo" size="4" />
	<Separator color="cyan" size="4" />
	<Separator color="orange" size="4" />
	<Separator color="crimson" size="4" />
</Flex>
```

----------------------------------------

TITLE: Displaying Keyboard Input with Radix UI Kbd Component in JSX
DESCRIPTION: This JSX snippet demonstrates how to render keyboard shortcuts using the Kbd component from Radix UI. The component represents keyboard inputs or hotkeys. There's a live preview setup indicated by `live=true` in the code block.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/kbd.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Kbd>Shift + Tab</Kbd>
```

----------------------------------------

TITLE: Applying Visual Variants to Segmented Control in JSX
DESCRIPTION: This example demonstrates the use of the 'variant' prop to control the visual style of the Segmented Control. It shows two variants: 'surface' and 'classic'.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/segmented-control.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Flex align="start" direction="column" gap="4">
	<SegmentedControl.Root defaultValue="inbox" variant="surface">
		<SegmentedControl.Item value="inbox">Inbox</SegmentedControl.Item>
		<SegmentedControl.Item value="drafts">Drafts</SegmentedControl.Item>
		<SegmentedControl.Item value="sent">Sent</SegmentedControl.Item>
	</SegmentedControl.Root>

	<SegmentedControl.Root defaultValue="inbox" variant="classic">
		<SegmentedControl.Item value="inbox">Inbox</SegmentedControl.Item>
		<SegmentedControl.Item value="drafts">Drafts</SegmentedControl.Item>
		<SegmentedControl.Item value="sent">Sent</SegmentedControl.Item>
	</SegmentedControl.Root>
</Flex>
```

----------------------------------------

TITLE: Progress Bar with Size Variations
DESCRIPTION: Shows how to use the size prop to control the visual dimensions of the Progress component across multiple sizes
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/progress.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="4" maxWidth="300px">
	<Progress value={25} size="1" />
	<Progress value={50} size="2" />
	<Progress value={75} size="3" />
</Flex>
```

----------------------------------------

TITLE: Displaying Text with Color Steps 11-12 in JSX
DESCRIPTION: This code shows how to apply steps 11 and 12 of the Radix color scale to text elements. It demonstrates using step 11 for low-contrast text and step 12 for high-contrast text across pink, slate, and gray colors.
SOURCE: https://github.com/radix-ui/website/blob/main/data/colors/docs/palette-composition/understanding-the-scale.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
<Flex gap="2" direction="column" my="5">
	<Text size="3" style={{ color: "var(--pink-11)" }}>
		This text is Pink 11
	</Text>
	<Text size="3" style={{ color: "var(--slate-11)" }}>
		This text is Slate 11
	</Text>
	<Text size="3" style={{ color: "var(--gray-11)" }}>
		This text is Gray 11
	</Text>
	<Text size="3" weight="bold" style={{ color: "var(--pink-12)" }}>
		This text is Pink 12
	</Text>
	<Text size="3" weight="bold" style={{ color: "var(--slate-12)" }}>
		This text is Slate 12
	</Text>
	<Text size="3" weight="bold" style={{ color: "var(--gray-12)" }}>
		This text is Gray 12
	</Text>
</Flex>
```

----------------------------------------

TITLE: TextArea Resize Controls
DESCRIPTION: Shows different resize options for the TextArea using the resize prop to control resizing behavior.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/text-area.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="3" maxWidth="250px">
	<TextArea resize="none" placeholder="Search the docs…" />
	<TextArea resize="vertical" placeholder="Search the docs…" />
	<TextArea resize="horizontal" placeholder="Search the docs…" />
	<TextArea resize="both" placeholder="Search the docs…" />
</Flex>
```

----------------------------------------

TITLE: Truncating Quote Text in JSX
DESCRIPTION: This example illustrates the use of the `truncate` prop with the Quote component to manage text overflow by introducing an ellipsis. The component is contained within a Flex component restricting maximum width, demonstrating responsive design capabilities within a React project using Radix UI. Dependencies include Radix UI's Flex and Quote components, and the setup occurs in an environment supporting JSX. The code handles long text inputs cleanly by truncating overflow, thus ensuring visual consistency.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/quote.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Flex maxWidth=\"300px\">\n\t<Quote truncate>\n\t\tThe goal of typography is to relate font size, line height, and line width\n\t\tin a proportional way that maximizes beauty and makes reading easier and\n\t\tmore pleasant.\n\t</Quote>\n</Flex>
```

----------------------------------------

TITLE: CSS for Collision-Aware HoverCard Animation
DESCRIPTION: This CSS snippet defines collision-aware animations for the 'HoverCardContent' class, using the `data-side` attribute to determine the animation direction. It sets a base animation duration and timing function and then defines separate animations ('slideUp' and 'slideDown') for the top and bottom sides, creating a slide-in effect from the appropriate direction.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/hover-card.mdx#2025-04-21_snippet_8

LANGUAGE: css
CODE:
```
/* styles.css */
.HoverCardContent {
	animation-duration: 0.6s;
	animation-timing-function: cubic-bezier(0.16, 1, 0.3, 1);
}
.HoverCardContent[__data-side="top"__] {
	animation-name: slideUp;
}
.HoverCardContent[__data-side="bottom"__] {
	animation-name: slideDown;
}

@keyframes slideUp {
	from {
		opacity: 0;
		transform: translateY(10px);
	}
	to {
		opacity: 1;
		transform: translateY(0);
	}
}

@keyframes slideDown {
	from {
		opacity: 0;
		transform: translateY(-10px);
	}
	to {
		opacity: 1;
		transform: translateY(0);
	}
}
```

----------------------------------------

TITLE: Popover Collision Animations (CSS)
DESCRIPTION: This CSS code defines animations for the PopoverContent based on the `data-side` attribute. It uses keyframes to create slideUp and slideDown animations that are triggered when the popover is positioned at the top or bottom of the trigger element, respectively.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/popover.mdx#2025-04-21_snippet_7

LANGUAGE: css
CODE:
```
"/* styles.css */
.PopoverContent {
	animation-duration: 0.6s;
	animation-timing-function: cubic-bezier(0.16, 1, 0.3, 1);
}
.PopoverContent[__data-side__=\"top\"] {
	animation-name: slideUp;
}
.PopoverContent[__data-side__=\"bottom\"] {
	animation-name: slideDown;
}

@keyframes slideDown {
	from {
		opacity: 0;
		transform: translateY(-10px);
	}
	to {
		opacity: 1;
		transform: translateY(0);
	}
}

@keyframes slideUp {
	from {
		opacity: 0;
		transform: translateY(10px);
	}
	to {
		opacity: 1;
		transform: translateY(0);
	}
}"
```

----------------------------------------

TITLE: Styling Advanced Animations for Radix UI Navigation Menu
DESCRIPTION: This CSS code defines the styles and animations for the `NavigationMenuContent` and `NavigationMenuViewport` components, enabling advanced animation effects. It uses keyframes to define the enter and exit animations based on the direction of movement.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/navigation-menu.mdx#2025-04-21_snippet_10

LANGUAGE: css
CODE:
```
/* styles.css */
.NavigationMenuContent {
	position: absolute;
	top: 0;
	left: 0;
	animation-duration: 250ms;
	animation-timing-function: ease;
}
.NavigationMenuContent[__data-motion__="from-start"] {
	animation-name: enterFromLeft;
}
.NavigationMenuContent[__data-motion__="from-end"] {
	animation-name: enterFromRight;
}
.NavigationMenuContent[__data-motion__="to-start"] {
	animation-name: exitToLeft;
}
.NavigationMenuContent[__data-motion__="to-end"] {
	animation-name: exitToRight;
}

.NavigationMenuViewport {
	position: relative;
	width: var(__--radix-navigation-menu-viewport-width__);
	height: var(__--radix-navigation-menu-viewport-height__);
	transition:
		width,
		height,
		250ms ease;
}

@keyframes enterFromRight {
	from {
		opacity: 0;
		transform: translateX(200px);
	}
	to {
		opacity: 1;
		transform: translateX(0);
	}
}

@keyframes enterFromLeft {
	from {
		opacity: 0;
		transform: translateX(-200px);
	}
	to {
		opacity: 1;
		transform: translateX(0);
	}
}

@keyframes exitToRight {
	from {
		opacity: 1;
		transform: translateX(0);
	}
	to {
		opacity: 0;
		transform: translateX(200px);
	}
}

@keyframes exitToLeft {
	from {
		opacity: 1;
		transform: translateX(0);
	}
	to {
		opacity: 0;
		transform: translateX(-200px);
	}
}
```

----------------------------------------

TITLE: Adding ThemePanel to React Application
DESCRIPTION: Example of how to add the ThemePanel component to a React application for real-time theme previewing.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/overview/getting-started.mdx#2025-04-21_snippet_7

LANGUAGE: jsx
CODE:
```
import { Theme, ThemePanel } from "@radix-ui/themes";

export default function () {
	return (
		<Theme>
			<MyApp />
			<ThemePanel />
		</Theme>
	);
}
```

----------------------------------------

TITLE: Adding Labels to ContextMenu in React
DESCRIPTION: Example showing how to add descriptive labels to sections in a ContextMenu using the Label component.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/context-menu.mdx#2025-04-21_snippet_14

LANGUAGE: jsx
CODE:
```
<ContextMenu.Root>
	<ContextMenu.Trigger>…</ContextMenu.Trigger>
	<ContextMenu.Portal>
		<ContextMenu.Content>
			<ContextMenu.Label>Label</ContextMenu.Label>
			<ContextMenu.Item>…</ContextMenu.Item>
			<ContextMenu.Item>…</ContextMenu.Item>
			<ContextMenu.Item>…</ContextMenu.Item>
		</ContextMenu.Content>
	</ContextMenu.Portal>
</ContextMenu.Root>
```

----------------------------------------

TITLE: Rendering Surface Variant Buttons with Various Color Options in JSX
DESCRIPTION: This code demonstrates how to implement surface variant buttons with various color options from the Radix color palette. It shows the usage of steps 6-8 for borders on interactive components.
SOURCE: https://github.com/radix-ui/website/blob/main/data/colors/docs/palette-composition/understanding-the-scale.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Grid columns="7" gap="3" my="5">
	<Button variant="surface" color="gold">
		Gold
	</Button>
	<Button variant="surface" color="bronze">
		Bronze
	</Button>
	<Button variant="surface" color="brown">
		Brown
	</Button>
	<Button variant="surface" color="yellow">
		Yellow
	</Button>
	<Button variant="surface" color="amber">
		Amber
	</Button>
	<Button variant="surface" color="orange">
		Orange
	</Button>
	<Button variant="surface" color="tomato">
		Tomato
	</Button>
	<Button variant="surface" color="red">
		Red
	</Button>
	<Button variant="surface" color="ruby">
		Ruby
	</Button>
	<Button variant="surface" color="crimson">
		Crimson
	</Button>
	<Button variant="surface" color="pink">
		Pink
	</Button>
	<Button variant="surface" color="plum">
		Plum
	</Button>
	<Button variant="surface" color="purple">
		Purple
	</Button>
	<Button variant="surface" color="violet">
		Violet
	</Button>
	<Button variant="surface" color="iris">
		Iris
	</Button>
	<Button variant="surface" color="indigo">
		Indigo
	</Button>
	<Button variant="surface" color="blue">
		Blue
	</Button>
	<Button variant="surface" color="cyan">
		Cyan
	</Button>
	<Button variant="surface" color="teal">
		Teal
	</Button>
	<Button variant="surface" color="jade">
		Jade
	</Button>
	<Button variant="surface" color="green">
		Green
	</Button>
	<Button variant="surface" color="grass">
		Grass
	</Button>
	<Button variant="surface" color="lime">
		Lime
	</Button>
	<Button variant="surface" color="mint">
		Mint
	</Button>
	<Button variant="surface" color="sky">
		Sky
	</Button>
</Grid>
```

----------------------------------------

TITLE: Adjusting Radio Card Colors in React with Radix
DESCRIPTION: Explores the "color" prop, allowing developers to set the theme color of Radio Cards in Radix UI. The snippet highlights how different color options can be applied to visually distinguish cards according to user preference or branding requirements. Integration of Radix UI in a React project is necessary.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/radio-cards.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="3" maxWidth="200px">\n\t<RadioCards.Root defaultValue="1" color="indigo">\n\t\t<RadioCards.Item value="1">8-core CPU</RadioCards.Item>\n\t</RadioCards.Root>\n\n\t<RadioCards.Root defaultValue="1" color="cyan">\n\t\t<RadioCards.Item value="1">8-core CPU</RadioCards.Item>\n\t</RadioCards.Root>\n\n\t<RadioCards.Root defaultValue="1" color="orange">\n\t\t<RadioCards.Item value="1">8-core CPU</RadioCards.Item>\n\t</RadioCards.Root>\n\n\t<RadioCards.Root defaultValue="1" color="crimson">\n\t\t<RadioCards.Item value="1">8-core CPU</RadioCards.Item>\n\t</RadioCards.Root>\n</Flex>
```

----------------------------------------

TITLE: Applying Full Radius Theme to Multiple Components
DESCRIPTION: Shows how different components react to a 'full' radius theme setting, demonstrating contextual application of border-radius.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/radius.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Theme radius="full">
	<Flex align="center" gap="3">
		<Button>Save</Button>
		<Switch defaultChecked />
		<Checkbox defaultChecked />
	</Flex>
</Theme>
```

----------------------------------------

TITLE: Progress Bar with Controlled Value
DESCRIPTION: Demonstrates setting a specific progress value using the value prop
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/progress.mdx#2025-04-21_snippet_6

LANGUAGE: jsx
CODE:
```
<Progress value={75} />
```

----------------------------------------

TITLE: Rendering Soft Icon Buttons and Menu Items with Gray/Pink Colors in JSX
DESCRIPTION: This code demonstrates how to implement soft icon buttons and menu items using gray and pink colors from the Radix color scale. It shows proper usage of steps 3-5 for component backgrounds in normal, hover, and selected states.
SOURCE: https://github.com/radix-ui/website/blob/main/data/colors/docs/palette-composition/understanding-the-scale.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Flex wrap="wrap" gap="5" my="5">
  <IconButton variant="soft" color="gray"><PlusIcon /></IconButton>
  <IconButton variant="soft" color="pink"><PlusIcon /></IconButton>

{' '}

<Flex direction="column" maxWidth="max-content">
	<MenuItemButton color="gray" data-state="active">
		Menu item
	</MenuItemButton>
	<MenuItemButton color="gray">Second menu item</MenuItemButton>
	<MenuItemButton color="gray">Third menu item</MenuItemButton>
</Flex>

  <Flex direction="column" maxWidth="max-content">
    <MenuItemButton color="pink" data-state="active">
      Menu item
    </MenuItemButton>
    <MenuItemButton color="pink">
      Second menu item
    </MenuItemButton>
    <MenuItemButton color="pink">
      Third menu item
    </MenuItemButton>
  </Flex>
</Flex>
```

----------------------------------------

TITLE: Customizing Avatar Fallback Elements in a Flex Container
DESCRIPTION: This snippet demonstrates how to customize the fallback element displayed by Avatar components using the 'fallback' prop. It includes examples of different fallback characters as well as a custom SVG fallback element.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/avatar.mdx#2025-04-21_snippet_6

LANGUAGE: jsx
CODE:
```
<Flex gap="2">
	<Avatar fallback="A" />
	<Avatar fallback="AB" />
	<Avatar
		fallback={
			<Box width="24px" height="24px">
				<svg viewBox="0 0 64 64" fill="currentColor">
					<path d="M41.5 14c4.687 0 8.5 4.038 8.5 9s-3.813 9-8.5 9S33 27.962 33 23 36.813 14 41.5 14zM56.289 43.609C57.254 46.21 55.3 49 52.506 49c-2.759 0-11.035 0-11.035 0 .689-5.371-4.525-10.747-8.541-13.03 2.388-1.171 5.149-1.834 8.07-1.834C48.044 34.136 54.187 37.944 56.289 43.609zM37.289 46.609C38.254 49.21 36.3 52 33.506 52c-5.753 0-17.259 0-23.012 0-2.782 0-4.753-2.779-3.783-5.392 2.102-5.665 8.245-9.472 15.289-9.472S35.187 40.944 37.289 46.609zM21.5 17c4.687 0 8.5 4.038 8.5 9s-3.813 9-8.5 9S13 30.962 13 26 16.813 17 21.5 17z" />
				</svg>
			</Box>
		}
	/>
</Flex>
```

----------------------------------------

TITLE: Progress Bar with Variant Styles
DESCRIPTION: Demonstrates different visual styles using the variant prop for Progress components
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/progress.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="4" maxWidth="300px">
	<Progress value={25} variant="classic" />
	<Progress value={50} variant="surface" />
	<Progress value={75} variant="soft" />
</Flex>
```

----------------------------------------

TITLE: Constraining Tooltip Content Size with CSS Variables
DESCRIPTION: Demonstrates using Radix UI's exposed CSS variables to dynamically size tooltip content based on trigger dimensions and viewport constraints
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/tooltip.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
// index.jsx
import { Tooltip } from "radix-ui";
import "./styles.css";

export default () => (
	<Tooltip.Root>
		<Tooltip.Trigger>…</Tooltip.Trigger>
		<Tooltip.Portal>
			<Tooltip.Content __className__="TooltipContent" sideOffset={5}>
				…
			</Tooltip.Content>
		</Tooltip.Portal>
	</Tooltip.Root>
);
```

LANGUAGE: css
CODE:
```
/* styles.css */
.TooltipContent {
	width: var(__--radix-tooltip-trigger-width__);
	max-height: var(__--radix-tooltip-content-available-height__);
}
```

----------------------------------------

TITLE: HoverCard Content with Size Constraints in React JSX
DESCRIPTION: This JSX snippet demonstrates how to use the Radix UI HoverCard component and style its content to constrain its width and maximum height using CSS custom properties. It imports the HoverCard component from 'radix-ui' and renders a HoverCard.Root containing a Trigger and a Content component, applying the 'HoverCardContent' class to the Content component.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/hover-card.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
import { HoverCard } from "radix-ui";
import "./styles.css";

export default () => (
	<HoverCard.Root>
		<HoverCard.Trigger>…</HoverCard.Trigger>
		<HoverCard.Portal>
			<HoverCard.Content __className__="HoverCardContent" sideOffset={5}>
				…
			</HoverCard.Content>
		</HoverCard.Portal>
	</HoverCard.Root>
);
```

----------------------------------------

TITLE: Setting Visual Style Variants for Code Snippets in JSX
DESCRIPTION: This snippet demonstrates the use of the 'variant' prop in the 'Code' component to specify different visual styles like solid, soft, outline, and ghost.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/code.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Flex direction="column" align="start" gap="2">
	<Code variant="solid">console.log()</Code>
	<Code variant="soft">console.log()</Code>
	<Code variant="outline">console.log()</Code>
	<Code variant="ghost">console.log()</Code>
</Flex>
```

----------------------------------------

TITLE: Handling Disabled Items with Data Attributes
DESCRIPTION: Shows how to apply custom styles to disabled Select items using data-disabled attribute
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/select.mdx#2025-04-21_snippet_6

LANGUAGE: jsx
CODE:
```
import { Select } from "radix-ui";
import "./styles.css";

export default () => (
	<Select.Root>
		<Select.Trigger>…</Select.Trigger>
		<Select.Portal>
			<Select.Content>
				<Select.Viewport>
					<Select.Item __className__="SelectItem" __disabled__>
						…
					</Select.Item>
					<Select.Item>…</Select.Item>
					<Select.Item>…</Select.Item>
				</Select.Viewport>
			</Select.Content>
		</Select.Portal>
	</Select.Root>
);
```

LANGUAGE: css
CODE:
```
.SelectItem[__data-disabled__] {
	color: "gainsboro";
}
```

----------------------------------------

TITLE: Implementing Advanced Animation with Radix UI
DESCRIPTION: This code snippet demonstrates how to implement advanced animations for the Radix UI Navigation Menu content. It uses CSS variables and data attributes to control the animation based on the enter/exit direction, creating smooth overlapping effects. This requires CSS styling to define the animations.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/navigation-menu.mdx#2025-04-21_snippet_9

LANGUAGE: jsx
CODE:
```
// index.jsx
import { NavigationMenu } from "radix-ui";
import "./styles.css";

export default () => (
	<NavigationMenu.Root>
		<NavigationMenu.List>
			<NavigationMenu.Item>
				<NavigationMenu.Trigger>Item one</NavigationMenu.Trigger>
				<NavigationMenu.Content __className__="NavigationMenuContent">
					Item one content
				</NavigationMenu.Content>
			</NavigationMenu.Item>
			<NavigationMenu.Item>
				<NavigationMenu.Trigger>Item two</NavigationMenu.Trigger>
				<NavigationMenu.Content __className__="NavigationMenuContent">
					Item two content
				</NavigationMenu.Content>
			</NavigationMenu.Item>
		</NavigationMenu.List>

		<NavigationMenu.Viewport __className__="NavigationMenuViewport" />
	</NavigationMenu.Root>
);
```

----------------------------------------

TITLE: Importing Radix Colors with emotion
DESCRIPTION: This snippet shows the import statements required to use Radix Colors with emotion.  It imports the color scales and the ThemeProvider from `@emotion/react`, and `styled` from `@emotion/styled`.
SOURCE: https://github.com/radix-ui/website/blob/main/data/colors/docs/overview/usage.mdx#2025-04-21_snippet_3

LANGUAGE: javascript
CODE:
```
import {
	gray,
	blue,
	red,
	green,
	grayDark,
	blueDark,
	redDark,
	greenDark,
} from "@radix-ui/colors";
import { ThemeProvider } from "@emotion/react";
import styled from "@emotion/styled";
```

----------------------------------------

TITLE: Renaming Radix UI Color Scales in CSS
DESCRIPTION: This snippet demonstrates how to import Radix UI color scales via CDN and rename them using CSS custom properties. It shows how to map standard scales like 'slate' to more intuitive names like 'gray' or create brand-specific names like 'blurple'.
SOURCE: https://github.com/radix-ui/website/blob/main/data/colors/docs/overview/aliasing.mdx#2025-04-21_snippet_4

LANGUAGE: css
CODE:
```
/*
 * Note: Importing from the CDN in production is not recommended.
 * It's intended for prototyping only.
 */
@import "https://cdn.jsdelivr.net/npm/@radix-ui/colors@latest/slate.css";
@import "https://cdn.jsdelivr.net/npm/@radix-ui/colors@latest/sky.css";
@import "https://cdn.jsdelivr.net/npm/@radix-ui/colors@latest/grass.css";
@import "https://cdn.jsdelivr.net/npm/@radix-ui/colors@latest/violet.css";
@import "https://cdn.jsdelivr.net/npm/@radix-ui/colors@latest/crimson.css";

:root {
	--gray-1: var(--slate-1);
	--gray-2: var(--slate-2);

	--blue-1: var(--sky-1);
	--blue-2: var(--sky-2);

	--green-1: var(--grass-1);
	--green-2: var(--grass-2);

	--blurple-1: var(--violet-1);
	--blurple-2: var(--violet-2);

	--caribbean-sunset-1: var(--crimson-1);
	--caribbean-sunset-2: var(--crimson-2);
}
```

----------------------------------------

TITLE: Implementing Shadow Tokens with CSS Variables
DESCRIPTION: Demonstrates the usage of shadow CSS variables in the Radix UI theme system. The tokens range from subtle inset shadows (shadow-1) to prominent overlay shadows (shadow-6), allowing consistent shadow implementation across different component types.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/shadows.mdx#2025-04-21_snippet_0

LANGUAGE: css
CODE:
```
/* Inset shadow */
var(--shadow-1);

/* Shadows for variant="classic" panels, like Card */
var(--shadow-2);
var(--shadow-3);

/* Shadows for smaller overlay panels, like Hover Card and Popover */
var(--shadow-4);
var(--shadow-5);

/* Shadows for larger overlay panels, like Dialog */
var(--shadow-6);
```

----------------------------------------

TITLE: Varying Badge Variants in JSX
DESCRIPTION: This JSX code illustrates using the 'variant' prop to style Badge elements with different visual appearances, including solid, soft, surface, and outline variants. It helps in quickly changing the UI representation of badges in a Flex component layout.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/badge.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Flex gap="2">\n	<Badge variant="solid" color="indigo">\n		New\n	</Badge>\n	<Badge variant="soft" color="indigo">\n		New\n	</Badge>\n	<Badge variant="surface" color="indigo">\n		New\n	</Badge>\n	<Badge variant="outline" color="indigo">\n		New\n	</Badge>\n</Flex>
```

----------------------------------------

TITLE: Importing and Using unstable_OneTimePasswordField in Radix UI (TSX)
DESCRIPTION: Shows how to import and use the new `unstable_OneTimePasswordField` primitive from `radix-ui`. It illustrates the structure using `Root`, multiple `Input` components for individual characters, and a `HiddenInput` to provide a single value for form submission. This primitive is designed for multi-input one-time password fields and is currently unstable for preview testing.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/overview/releases.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { unstable_OneTimePasswordField as OneTimePasswordField } from "radix-ui";

export function Verify() {
	return (
		<OneTimePasswordField.Root>
			<OneTimePasswordField.Input />
			<OneTimePasswordField.Input />
			<OneTimePasswordField.Input />
			<OneTimePasswordField.Input />
			<OneTimePasswordField.Input />
			<OneTimePasswordField.Input />
			<OneTimePasswordField.HiddenInput />
		</OneTimePasswordField.Root>
	);
}
```

----------------------------------------

TITLE: Configuring Scroll Area Scrollbars in JSX
DESCRIPTION: This example shows how to use the 'scrollbars' prop to limit scrollable axes. It demonstrates both vertical-only and horizontal-only scrolling configurations.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/scroll-area.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Grid columns="2" gap="2">
	<ScrollArea type="always" scrollbars="vertical" style={{ height: 150 }}>
		<Flex p="2" pr="8" direction="column" gap="4">
			<Text size="2" trim="both">
				Three fundamental aspects of typography are legibility, readability, and
				aesthetics. Although in a non-technical sense "legible" and "readable"
				are often used synonymously, typographically they are separate but
				related concepts.
			</Text>

			<Text size="2" trim="both">
				Legibility describes how easily individual characters can be
				distinguished from one another. It is described by Walter Tracy as "the
				quality of being decipherable and recognisable". For instance, if a "b"
				and an "h", or a "3" and an "8", are difficult to distinguish at small
				sizes, this is a problem of legibility.
			</Text>
		</Flex>
	</ScrollArea>

	<ScrollArea type="always" scrollbars="horizontal" style={{ height: 150 }}>
		<Flex gap="4" p="2" width="700px">
			<Text size="2" trim="both">
				Three fundamental aspects of typography are legibility, readability, and
				aesthetics. Although in a non-technical sense "legible" and "readable"
				are often used synonymously, typographically they are separate but
				related concepts.
			</Text>

			<Text size="2" trim="both">
				Legibility describes how easily individual characters can be
				distinguished from one another. It is described by Walter Tracy as "the
				quality of being decipherable and recognisable". For instance, if a "b"
				and an "h", or a "3" and an "8", are difficult to distinguish at small
				sizes, this is a problem of legibility.
			</Text>
		</Flex>
	</ScrollArea>
</Flex>
```

----------------------------------------

TITLE: Long-form Content with Different Text Sizes in JSX
DESCRIPTION: Example showing how sizes 2-4 are designed to work well for long-form content, with different paragraph sizes demonstrating appropriate typography for extended text.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/text.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<Text as="p" mb="5" size="4">
  The goal of typography is to relate font size, line height, and line width in a proportional way that maximizes beauty and makes reading easier and more pleasant. The question is: What proportion(s) will give us the best results? The golden ratio is often observed in nature where beauty and utility intersect; perhaps we can use this "divine" proportion to enhance these attributes in our typography.
</Text>

<Text as="p" mb="5" size="3">
  The goal of typography is to relate font size, line height, and line width in a proportional way that maximizes beauty and makes reading easier and more pleasant. The question is: What proportion(s) will give us the best results? The golden ratio is often observed in nature where beauty and utility intersect; perhaps we can use this "divine" proportion to enhance these attributes in our typography.
</Text>

<Text as="p" size="2" color="gray">
  The goal of typography is to relate font size, line height, and line width in a proportional way that maximizes beauty and makes reading easier and more pleasant. The question is: What proportion(s) will give us the best results? The golden ratio is often observed in nature where beauty and utility intersect; perhaps we can use this "divine" proportion to enhance these attributes in our typography.
</Text>
```

----------------------------------------

TITLE: Defining Menubar RadioItem Props
DESCRIPTION: Props configuration for a radio-style menu item with unique value, disabled status, and selection event handling
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/menubar.mdx#2025-04-21_snippet_4

LANGUAGE: typescript
CODE:
```
{
  asChild?: boolean
  value: string
  disabled?: boolean
  onSelect?: (event: Event) => void
  textValue?: string
}
```

----------------------------------------

TITLE: Styling the Navigation Menu Indicator with CSS
DESCRIPTION: This CSS snippet styles the Navigation Menu Indicator, defining its background color, height, and transition properties. It also includes specific styling for horizontal orientation.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/navigation-menu.mdx#2025-04-21_snippet_5

LANGUAGE: css
CODE:
```
/* styles.css */
.NavigationMenuIndicator {
	background-color: grey;
}
.NavigationMenuIndicator[data-orientation="horizontal"] {
	height: 3px;
	transition:
		width,
		transform,
		250ms ease;
}
```

----------------------------------------

TITLE: Usage Example for Abstracted Select Component
DESCRIPTION: Shows how to use a custom abstracted Select component API that simplifies the implementation for developers by hiding the complexity of the underlying primitive parts.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/select.mdx#2025-04-21_snippet_12

LANGUAGE: jsx
CODE:
```
import { Select, SelectItem } from "./your-select";

export default () => (
	<Select defaultValue="2">
		<SelectItem value="1">Item 1</SelectItem>
		<SelectItem value="2">Item 2</SelectItem>
		<SelectItem value="3">Item 3</SelectItem>
	</Select>
);
```

----------------------------------------

TITLE: Switch Component with High Contrast in JSX
DESCRIPTION: Shows the Switch component with and without the highContrast prop for different colors, demonstrating increased color contrast in light mode.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/switch.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
<Grid rows="2" gapX="2" gapY="3" display="inline-grid" flow="column">
	<Switch color="indigo" defaultChecked />
	<Switch color="indigo" defaultChecked highContrast />
	<Switch color="cyan" defaultChecked />
	<Switch color="cyan" defaultChecked highContrast />
	<Switch color="orange" defaultChecked />
	<Switch color="orange" defaultChecked highContrast />
	<Switch color="crimson" defaultChecked />
	<Switch color="crimson" defaultChecked highContrast />
	<Switch color="gray" defaultChecked />
	<Switch color="gray" defaultChecked highContrast />
</Grid>
```

----------------------------------------

TITLE: Implementing Flexible Layouts with Radix UI Viewport
DESCRIPTION: This example demonstrates using the `NavigationMenu.Viewport` component to control where the `NavigationMenu.Content` is rendered. This is useful for designs requiring adjusted DOM structures or advanced animations, ensuring tab focus is automatically maintained.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/navigation-menu.mdx#2025-04-21_snippet_3

LANGUAGE: jsx
CODE:
```
<NavigationMenu.Root>
	<NavigationMenu.List>
		<NavigationMenu.Item>
			<NavigationMenu.Trigger>Item one</NavigationMenu.Trigger>
			<NavigationMenu.Content>Item one content</NavigationMenu.Content>
		</NavigationMenu.Item>
		<NavigationMenu.Item>
			<NavigationMenu.Trigger>Item two</NavigationMenu.Trigger>
			<NavigationMenu.Content>Item two content</NavigationMenu.Content>
		</NavigationMenu.Item>
	</NavigationMenu.List>

	{/* NavigationMenu.Content will be rendered here when active */}
	<NavigationMenu.Viewport />
</NavigationMenu.Root>
```

----------------------------------------

TITLE: Defining RadioItem Properties in React
DESCRIPTION: This snippet defines properties for the RadioItem component used in the context of a RadioGroup. It includes properties such as 'asChild', 'value', 'disabled', and 'onSelect', detailing their types, requirements, and descriptions for use in a React application.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/context-menu.mdx#2025-04-21_snippet_6

LANGUAGE: javascript
CODE:
```
<PropsTable
	data={[
		{
			name: "asChild",
			required: false,
			type: "boolean",
			default: "false",
			description: (
				<>Change the default rendered element for the one passed as a child,
				merging their props and behavior.<br /><br />Read our <a href="../guides/composition">Composition</a> guide for more details.</>
			),
		},
		{
			name: "value",
			type: "string",
			required: true,
			description: "The unique value of the item.",
		},
		{
			name: "disabled",
			type: "boolean",
			description: (
				<span>When <Code>true</Code>, prevents the user from interacting with the item.</span>
			),
		},
		{
			name: "onSelect",
			type: "(event: Event) => void",
			typeSimple: "function",
			description: (
				<span>Event handler called when the user selects an item (via mouse or keyboard). Calling <Code>event.preventDefault</Code> in this handler will prevent the context menu from closing when selecting that item.</span>
			),
		},
		{
			name: "textValue",
			type: "string",
			description: (
				<span>Optional text used for typeahead purposes. By default the typeahead behavior will use the <Code>.textContent</Code> of the item. Use this when the content is complex, or you have non-textual content inside.</span>
			),
		},
	]}/>
```

----------------------------------------

TITLE: Configuring Hover Card to Show Instantly
DESCRIPTION: Example demonstrating how to configure a Hover Card to open immediately by setting the openDelay prop to 0.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/hover-card.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
import { HoverCard } from "radix-ui";

export default () => (
	<HoverCard.Root __openDelay__={0}>
		<HoverCard.Trigger>…</HoverCard.Trigger>
		<HoverCard.Content>…</HoverCard.Content>
	</HoverCard.Root>
);
```

----------------------------------------

TITLE: Alpha Colors CSS Variables
DESCRIPTION: Shows the pattern for solid and alpha color variables
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/color.mdx#2025-04-21_snippet_7

LANGUAGE: css
CODE:
```
/* Solid colors */
var(--red-1);
var(--red-2);
...
var(--red-12);

/* Alpha colors */
var(--red-a1);
var(--red-a2);
...
var(--red-a12);
```

----------------------------------------

TITLE: Implementing Basic Badge with Colors in JSX
DESCRIPTION: This snippet demonstrates rendering a Flex container with Badge components using the Radix UI library. Each Badge is styled with a color prop, showcasing different states such as 'In progress', 'In review', and 'Complete'. Dependencies include Radix UI components such as Flex and Badge.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/badge.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Flex gap="2">\n	<Badge color="orange">In progress</Badge>\n	<Badge color="blue">In review</Badge>\n	<Badge color="green">Complete</Badge>\n</Flex>
```

----------------------------------------

TITLE: Remapping Color Tokens in Radix Themes CSS
DESCRIPTION: This CSS snippet demonstrates how to alias color tokens in Radix Themes. It remaps the 'ruby' color scale to 'red', allowing developers to use more generic color names while maintaining the original color values.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/color.mdx#2025-04-21_snippet_11

LANGUAGE: css
CODE:
```
.radix-themes {
	--red-1: var(--ruby-1);
	--red-2: var(--ruby-2);
	--red-3: var(--ruby-3);
	--red-4: var(--ruby-4);
	--red-5: var(--ruby-5);
	--red-6: var(--ruby-6);
	--red-7: var(--ruby-7);
	--red-8: var(--ruby-8);
	--red-9: var(--ruby-9);
	--red-10: var(--ruby-10);
	--red-11: var(--ruby-11);
	--red-12: var(--ruby-12);

	--red-a1: var(--ruby-a1);
	--red-a2: var(--ruby-a2);
	--red-a3: var(--ruby-a3);
	--red-a4: var(--ruby-a4);
	--red-a5: var(--ruby-a5);
	--red-a6: var(--ruby-a6);
	--red-a7: var(--ruby-a7);
	--red-a8: var(--ruby-a8);
	--red-a9: var(--ruby-a9);
	--red-a10: var(--ruby-a10);
	--red-a11: var(--ruby-a11);
	--red-a12: var(--ruby-a12);

	--red-surface: var(--ruby-surface);
	--red-indicator: var(--ruby-indicator);
	--red-track: var(--ruby-track);
	--red-contrast: var(--ruby-contrast);
}
```

----------------------------------------

TITLE: Installing Radix Themes using yarn
DESCRIPTION: Command to install the Radix Themes package using yarn package manager.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/overview/getting-started.mdx#2025-04-21_snippet_1

LANGUAGE: bash
CODE:
```
yarn add @radix-ui/themes
```

----------------------------------------

TITLE: Implementing Collision-Aware Animations for Menubar (CSS)
DESCRIPTION: This CSS snippet demonstrates how to create collision-aware animations for a Radix UI Menubar. It uses 'data-side' attributes to apply different animations based on the content's position, and defines 'slideUp' and 'slideDown' keyframe animations.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/menubar.mdx#2025-04-21_snippet_18

LANGUAGE: css
CODE:
```
/* styles.css */
.MenubarContent {
	animation-duration: 0.6s;
	animation-timing-function: cubic-bezier(0.16, 1, 0.3, 1);
}
.MenubarContent[__data-side="top"__] {
	animation-name: slideUp;
}
.MenubarContent[__data-side="bottom"__] {
	animation-name: slideDown;
}

@keyframes slideUp {
	from {
		opacity: 0;
		transform: translateY(10px);
	}
	to {
		opacity: 1;
		transform: translateY(0);
	}
}

@keyframes slideDown {
	from {
		opacity: 0;
		transform: translateY(-10px);
	}
	to {
		opacity: 1;
		transform: translateY(0);
	}
}
```

----------------------------------------

TITLE: Background Color CSS Variables
DESCRIPTION: Lists the CSS variables used for different types of backgrounds
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/color.mdx#2025-04-21_snippet_8

LANGUAGE: css
CODE:
```
/* Page background */
var(--color-background);

/* Panel backgrounds, such as cards, tables, popovers, dropdown menus, etc. */
var(--color-panel-solid);
var(--color-panel-translucent);

/* Form component backgrounds, such as text fields, checkboxes, select, etc. */
var(--color-surface);

/* Dialog overlays */
var(--color-overlay);
```

----------------------------------------

TITLE: Panel Background Theme Settings
DESCRIPTION: Shows how to set panel background appearance in Theme component
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/color.mdx#2025-04-21_snippet_9

LANGUAGE: jsx
CODE:
```
<Theme panelBackground="translucent">
	<MyApp />
</Theme>
```

LANGUAGE: jsx
CODE:
```
<Theme panelBackground="solid">
	<MyApp />
</Theme>
```

----------------------------------------

TITLE: Creating Horizontal Accordion with React
DESCRIPTION: This snippet illustrates how to create a horizontal accordion using Radix UI with the 'orientation' prop set to 'horizontal'. It is designed for creating multiple accordion items displaying horizontally. The expected outcome is a horizontal layout of accordion items.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/accordion.mdx#2025-04-21_snippet_7

LANGUAGE: jsx
CODE:
```
<Accordion.Root __orientation__="horizontal">
	<Accordion.Item value="item-1">…</Accordion.Item>
	<Accordion.Item value="item-2">…</Accordion.Item>
</Accordion.Root>
```

----------------------------------------

TITLE: Animating Chevron rotation with CSS
DESCRIPTION: This CSS snippet is responsible for the animation of the chevron icon within the accordion. It defines the rotation of the icon when the accordion trigger is in the open state. The output is a smoothly animated chevron that enhances the user interface. The key functionality includes using the 'transition' property for smooth animations.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/accordion.mdx#2025-04-21_snippet_6

LANGUAGE: css
CODE:
```
/* styles.css */
.AccordionChevron {
	transition: transform 300ms;
}
.AccordionTrigger[data-state="open"] > .AccordionChevron {
	transform: rotate(180deg);
}
```

----------------------------------------

TITLE: Basic Reset Component Examples
DESCRIPTION: Collection of examples showing how Reset component can be applied to various HTML elements to remove default browser styling while maintaining semantic meaning. Each example demonstrates the Reset wrapper around a specific HTML element.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/reset.mdx#2025-04-21_snippet_0

LANGUAGE: jsx
CODE:
```
<Reset>
	<a href="#">Anchor</a>
</Reset>
```

LANGUAGE: jsx
CODE:
```
<Reset>
	<abbr title="Abbreviation">ABR</abbr>
</Reset>
```

LANGUAGE: jsx
CODE:
```
<Reset>
	<address>Address</address>
</Reset>
```

----------------------------------------

TITLE: Implementing Vertical Slider
DESCRIPTION: Example showing how to create a vertically oriented slider using the orientation prop set to 'vertical'. Includes both JSX and corresponding CSS for styling.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/slider.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
// index.jsx
import { Slider } from "radix-ui";
import "./styles.css";

export default () => (
	<Slider.Root
		className="SliderRoot"
		defaultValue={[50]}
		__orientation__="vertical"
	>
		<Slider.Track className="SliderTrack">
			<Slider.Range className="SliderRange" />
		</Slider.Track>
		<Slider.Thumb className="SliderThumb" />
	</Slider.Root>
);
```

LANGUAGE: css
CODE:
```
/* styles.css */
.SliderRoot {
	position: relative;
	display: flex;
	align-items: center;
}
.SliderRoot[__data-orientation="vertical"__] {
	flex-direction: column;
	width: 20px;
	height: 100px;
}

.SliderTrack {
	position: relative;
	flex-grow: 1;
	background-color: grey;
}
.SliderTrack[__data-orientation="vertical"__] {
	width: 3px;
}

.SliderRange {
	position: absolute;
	background-color: black;
}
.SliderRange[__data-orientation="vertical"__] {
	width: 100%;
}

.SliderThumb {
	display: block;
	width: 20px;
	height: 20px;
	background-color: black;
}
```

----------------------------------------

TITLE: Advanced Typography Customization in CSS
DESCRIPTION: Shows how to customize advanced typographic features like font size multiplier, font style, letter spacing, and leading trim for headings in Radix Themes.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/typography.mdx#2025-04-21_snippet_8

LANGUAGE: css
CODE:
```
.radix-themes {
	--heading-font-family: "Untitled Sans", sans-serif;
	--heading-font-size-adjust: 1.05;
	--heading-font-style: normal;
	--heading-leading-trim-start: 0.42em;
	--heading-leading-trim-end: 0.38em;
	--heading-letter-spacing: -0.01em;
}
```

----------------------------------------

TITLE: Importing Separated Radix Theme CSS Files
DESCRIPTION: Examples showing how to import individual Radix Themes CSS files separately for more granular control over style precedence.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/overview/styling.mdx#2025-04-21_snippet_3

LANGUAGE: javascript
CODE:
```
import "@radix-ui/themes/tokens.css";
import "@radix-ui/themes/components.css";
import "@radix-ui/themes/utilities.css";
```

LANGUAGE: javascript
CODE:
```
import "@radix-ui/themes/layout/tokens.css";
import "@radix-ui/themes/layout/components.css";
import "@radix-ui/themes/layout/utilities.css";
```

----------------------------------------

TITLE: Accessing Radius CSS Variables
DESCRIPTION: Shows the CSS variables available for accessing radius values in custom styling, including responsive radius factors and special radius calculations.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/radius.mdx#2025-04-21_snippet_3

LANGUAGE: css
CODE:
```
/* Radius values that automatically respond to the radius factor */
var(--radius-1);
var(--radius-2);
var(--radius-3);
var(--radius-4);
var(--radius-5);
var(--radius-6);

/* A multiplier that controls the theme radius */
var(--radius-factor);

/*
 * A variable used to calculate a fully rounded radius.
 * Usually used within a CSS `max()` function.
 */
var(--radius-full);

/*
 * A variable used to calculate radius of a thumb element.
 * Usually used within a CSS `max()` function.
 */
var(--radius-thumb);
```

----------------------------------------

TITLE: Using Scaling Factor CSS Variable in Custom Components
DESCRIPTION: Example of how to use the --scaling CSS variable to create custom components that respond to the theme's scaling factor, ensuring consistency with the rest of the application.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/theme/spacing.mdx#2025-04-21_snippet_2

LANGUAGE: css
CODE:
```
.MyCustomComponent {
	width: calc(200px * var(--scaling));
}
```

----------------------------------------

TITLE: HoverCard Content with Origin-Aware Animation in React JSX
DESCRIPTION: This JSX snippet demonstrates the basic structure of a Radix UI HoverCard component in React, similar to the previous examples, but focusing on integration with origin-aware animations. It imports the HoverCard component and includes a Trigger and Content element.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/hover-card.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
import { HoverCard } from "radix-ui";
import "./styles.css";

export default () => (
	<HoverCard.Root>
		<HoverCard.Trigger>…</HoverCard.Trigger>
		<HoverCard.Content __className__="HoverCardContent">…</HoverCard.Content>
	</HoverCard.Root>
);
```

----------------------------------------

TITLE: Adding labels to Menubar sections (Radix UI, React)
DESCRIPTION: This example demonstrates the usage of `Menubar.Label` to add labels to sections within a `Menubar.Content` component.  This improves the organization and clarity of the menu.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/menubar.mdx#2025-04-21_snippet_9

LANGUAGE: jsx
CODE:
```
<Menubar.Root>
	<Menubar.Menu>
		<Menubar.Trigger>…</Menubar.Trigger>
		<Menubar.Portal>
			<Menubar.Content>
				<Menubar.Label>Label</Menubar.Label>
				<Menubar.Item>…</Menubar.Item>
				<Menubar.Item>…</Menubar.Item>
				<Menubar.Item>…</Menubar.Item>
			</Menubar.Content>
		</Menubar.Portal>
	</Menubar.Menu>
</Menubar.Root>
```

----------------------------------------

TITLE: Applying Color to Select Component in React
DESCRIPTION: This snippet demonstrates how to use the 'color' prop on Trigger and Content to assign specific color values to the Select component. It shows examples with indigo, cyan, orange, and crimson colors.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/select.mdx#2025-04-21_snippet_4

LANGUAGE: jsx
CODE:
```
<Flex gap="3">
	<Select.Root defaultValue="apple">
		<Select.Trigger color="indigo" variant="soft" />
		<Select.Content color="indigo">
			<Select.Item value="apple">Apple</Select.Item>
			<Select.Item value="orange">Orange</Select.Item>
		</Select.Content>
	</Select.Root>

	<Select.Root defaultValue="apple">
		<Select.Trigger color="cyan" variant="soft" />
		<Select.Content color="cyan">
			<Select.Item value="apple">Apple</Select.Item>
			<Select.Item value="orange">Orange</Select.Item>
		</Select.Content>
	</Select.Root>

	<Select.Root defaultValue="apple">
		<Select.Trigger color="orange" variant="soft" />
		<Select.Content color="orange">
			<Select.Item value="apple">Apple</Select.Item>
			<Select.Item value="orange">Orange</Select.Item>
		</Select.Content>
	</Select.Root>

	<Select.Root defaultValue="apple">
		<Select.Trigger color="crimson" variant="soft" />
		<Select.Content color="crimson">
			<Select.Item value="apple">Apple</Select.Item>
			<Select.Item value="orange">Orange</Select.Item>
		</Select.Content>
	</Select.Root>
</Flex>
```

----------------------------------------

TITLE: Using Radix Colors with vanilla-extract
DESCRIPTION: This snippet demonstrates how to use Radix Colors with vanilla-extract. It imports color scales, creates light and dark themes, and applies the colors to a styled button component. Requires `@vanilla-extract/css` and `@radix-ui/colors` packages.
SOURCE: https://github.com/radix-ui/website/blob/main/data/colors/docs/overview/usage.mdx#2025-04-21_snippet_4

LANGUAGE: javascript
CODE:
```
import {
	gray,
	blue,
	red,
	green,
	grayDark,
	blueDark,
	redDark,
	greenDark,
} from "@radix-ui/colors";
import { createTheme } from "@vanilla-extract/css";

export const [theme, vars] = createTheme({
	colors: {
		...gray,
		...blue,
		...red,
		...green,
	},
});

export const darkTheme = createTheme(vars, {
	colors: {
		...grayDark,
		...blueDark,
		...redDark,
		...greenDark,
	},
});

// Use the colors in your styles
export const styles = {
	button: style({
		backgroundColor: vars.colors.blue4,
		color: vars.colors.blue11,
		borderColor: vars.colors.blue7,
		":hover": {
			backgroundColor: vars.colors.blue5,
			borderColor: vars.colors.blue8,
		},
	}),
};

// Apply your theme to it
export default function App() {
	return (
		<body className={theme}>
			<button className={styles.button}>Radix Colors</button>
		</body>
	);
}
```

----------------------------------------

TITLE: Constraining Menubar Content Size (CSS)
DESCRIPTION: This CSS snippet demonstrates how to use Radix UI's custom properties to constrain the width and height of the Menubar content. It uses '--radix-menubar-trigger-width' and '--radix-menubar-content-available-height' to match the content size to the trigger and viewport.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/menubar.mdx#2025-04-21_snippet_14

LANGUAGE: css
CODE:
```
/* styles.css */
.MenubarContent {
	width: var(__--radix-menubar-trigger-width__);
	max-height: var(__--radix-menubar-content-available-height__);
}
```

----------------------------------------

TITLE: Adding separators to Menubar (Radix UI, React)
DESCRIPTION: This example demonstrates how to use the `Menubar.Separator` component to add visual separators between menu items.  The separators help organize the menu content and improve readability.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/menubar.mdx#2025-04-21_snippet_8

LANGUAGE: jsx
CODE:
```
<Menubar.Root>
	<Menubar.Menu>
		<Menubar.Trigger>…</Menubar.Trigger>
		<Menubar.Portal>
			<Menubar.Content>
				<Menubar.Item>…</Menubar.Item>
				<Menubar.Separator />
				<Menubar.Item>…</Menubar.Item>
				<Menubar.Separator />
				<Menubar.Item>…</Menubar.Item>
			</Menubar.Content>
		</Menubar.Portal>
	</Menubar.Menu>
</Menubar.Root>
```

----------------------------------------

TITLE: Tabs Size Customization Example
DESCRIPTION: Shows how to implement different sized tab lists using the size prop. Demonstrates two variations with size='1' and size='2'.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/tabs.mdx#2025-04-21_snippet_1

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="4" pb="2">
	<Tabs.Root defaultValue="account">
		<Tabs.List size="1">
			<Tabs.Trigger value="account">Account</Tabs.Trigger>
			<Tabs.Trigger value="documents">Documents</Tabs.Trigger>
			<Tabs.Trigger value="settings">Settings</Tabs.Trigger>
		</Tabs.List>
	</Tabs.Root>

	<Tabs.Root defaultValue="account">
		<Tabs.List size="2">
			<Tabs.Trigger value="account">Account</Tabs.Trigger>
			<Tabs.Trigger value="documents">Documents</Tabs.Trigger>
			<Tabs.Trigger value="settings">Settings</Tabs.Trigger>
		</Tabs.List>
	</Tabs.Root>
</Flex>
```

----------------------------------------

TITLE: Configuring Select Component Radius in React
DESCRIPTION: This snippet demonstrates how to use the 'radius' prop to assign specific radius values to the Select component. It shows examples with 'none', 'large', and 'full' radius values.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/select.mdx#2025-04-21_snippet_6

LANGUAGE: jsx
CODE:
```
<Flex gap="3">
	<Select.Root defaultValue="apple">
		<Select.Trigger radius="none" />
		<Select.Content>
			<Select.Item value="apple">Apple</Select.Item>
			<Select.Item value="orange">Orange</Select.Item>
		</Select.Content>
	</Select.Root>

	<Select.Root defaultValue="apple">
		<Select.Trigger radius="large" />
		<Select.Content>
			<Select.Item value="apple">Apple</Select.Item>
			<Select.Item value="orange">Orange</Select.Item>
		</Select.Content>
	</Select.Root>

	<Select.Root defaultValue="apple">
		<Select.Trigger radius="full" />
		<Select.Content>
			<Select.Item value="apple">Apple</Select.Item>
			<Select.Item value="orange">Orange</Select.Item>
		</Select.Content>
	</Select.Root>
</Flex>
```

----------------------------------------

TITLE: Installing Radix UI Navigation Menu Package
DESCRIPTION: Command to install the Navigation Menu component package via npm.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/navigation-menu.mdx#2025-04-21_snippet_0

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-navigation-menu
```

----------------------------------------

TITLE: Constraining Menubar Content Size (JSX)
DESCRIPTION: This code snippet shows how to constrain the size of the Menubar content using CSS custom properties. It demonstrates the use of the 'className' prop to apply custom styles to the Menubar.Content component.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/menubar.mdx#2025-04-21_snippet_13

LANGUAGE: jsx
CODE:
```
// index.jsx
import { Menubar } from "radix-ui";
import "./styles.css";

export default () => (
	<Menubar.Root>
		<Menubar.Trigger>…</Menubar.Trigger>
		<Menubar.Portal>
			<Menubar.Content __className__="MenubarContent" sideOffset={5}>
				…
			</Menubar.Content>
		</Menubar.Portal>
	</Menubar.Root>
);
```

----------------------------------------

TITLE: Adding Separators to DropdownMenu in React
DESCRIPTION: This snippet demonstrates how to use the Separator part to add separators between items in a DropdownMenu.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/dropdown-menu.mdx#2025-04-21_snippet_9

LANGUAGE: jsx
CODE:
```
<DropdownMenu.Root>
	<DropdownMenu.Trigger>…</DropdownMenu.Trigger>
	<DropdownMenu.Portal>
		<DropdownMenu.Content>
			<DropdownMenu.Item>…</DropdownMenu.Item>
			<DropdownMenu.Separator />
			<DropdownMenu.Item>…</DropdownMenu.Item>
			<DropdownMenu.Separator />
			<DropdownMenu.Item>…</DropdownMenu.Item>
		</DropdownMenu.Content>
	</DropdownMenu.Portal>
</DropdownMenu.Root>
```

----------------------------------------

TITLE: Text Component with Pretty Wrapping in JSX
DESCRIPTION: Example using the 'wrap' prop set to 'pretty' to implement the experimental CSS text-wrap:pretty feature, which improves typography by preventing orphaned words at the end of paragraphs.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/text.mdx#2025-04-21_snippet_13

LANGUAGE: jsx
CODE:
```
<Flex maxWidth="270px">
	<Text wrap="pretty">
		The goal of typography is to relate font size, line height, and line width
		in a proportional way that maximizes beauty and makes reading easier and
		more pleasant.
	</Text>
</Flex>
```

----------------------------------------

TITLE: Preventing Thumb Overlap
DESCRIPTION: Shows how to implement a minimum distance between thumbs using the minStepsBetweenThumbs prop, ensuring thumbs cannot have equal values.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/slider.mdx#2025-04-21_snippet_5

LANGUAGE: jsx
CODE:
```
import { Slider } from "radix-ui";

export default () => (
	<Slider.Root defaultValue={[25, 75]} step={10} __minStepsBetweenThumbs__={1}>
		<Slider.Track>
			<Slider.Range />
		</Slider.Track>
		<Slider.Thumb />
		<Slider.Thumb />
	</Slider.Root>
);
```

----------------------------------------

TITLE: Importing CSS Files in Next.js Layout
DESCRIPTION: Example showing the correct order for importing Radix Themes and custom CSS files in Next.js layout files. Note that Next.js 13.0-14.1 has issues with CSS import order.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/overview/styling.mdx#2025-04-21_snippet_0

LANGUAGE: javascript
CODE:
```
import "@radix-ui/themes/styles.css";
import "./my-styles.css";
```

----------------------------------------

TITLE: HoverCard Component in React JSX
DESCRIPTION: This JSX snippet presents the Radix UI HoverCard component with a basic structure including a Trigger and Content section. This example sets the stage for later custom animations that change based on where the card appears in relation to the trigger element.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/hover-card.mdx#2025-04-21_snippet_7

LANGUAGE: jsx
CODE:
```
import { HoverCard } from "radix-ui";
import "./styles.css";

export default () => (
	<HoverCard.Root>
		<HoverCard.Trigger>…</HoverCard.Trigger>
		<HoverCard.Content __className__="HoverCardContent">…</HoverCard.Content>
	</HoverCard.Root>
);
```

----------------------------------------

TITLE: Styling Radio Cards with Variants in Radix UI
DESCRIPTION: Demonstrates the usage of the "variant" prop to apply different visual styles to Radio Cards. This example shows how developers can adjust the style of their components to match various design themes or requirements using predefined variant styles in Radix UI. Requires Radix UI setup in a React environment.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/radio-cards.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Flex direction="column" gap="3" maxWidth="200px">\n\t<RadioCards.Root variant="surface">\n\t\t<RadioCards.Item value="1">8-core CPU</RadioCards.Item>\n\t</RadioCards.Root>\n\n\t<RadioCards.Root variant="classic">\n\t\t<RadioCards.Item value="1">8-core CPU</RadioCards.Item>\n\t</RadioCards.Root>\n</Flex>
```

----------------------------------------

TITLE: Context Menu Visual Styling with Variants
DESCRIPTION: Shows different visual styles for context menus using the `variant` prop, demonstrating how to customize the appearance between solid and soft variants.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/context-menu.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Grid columns="2" gap="3">\n\t<ContextMenu.Root>\n\t\t<ContextMenu.Trigger>\n\t\t\t<RightClickZone title="Solid" />\n\t\t</ContextMenu.Trigger>\n\t\t<ContextMenu.Content variant="solid">\n\t\t\t<ContextMenu.Item shortcut="⌘ E">Edit</ContextMenu.Item>\n\t\t\t<ContextMenu.Item shortcut="⌘ D">Duplicate</ContextMenu.Item>\n\t\t\t<ContextMenu.Separator />\n\t\t\t<ContextMenu.Item shortcut="⌘ N">Archive</ContextMenu.Item>\n\n\t\t\t<ContextMenu.Separator />\n\t\t\t<ContextMenu.Item shortcut="⌘ ⌫" color="red">\n\t\t\t\tDelete\n\t\t\t</ContextMenu.Item>\n\t\t</ContextMenu.Content>\n\t</ContextMenu.Root>\n\n\t<ContextMenu.Root>\n\t\t<ContextMenu.Trigger>\n\t\t\t<RightClickZone title="Soft" />\n\t\t</ContextMenu.Trigger>\n\t\t<ContextMenu.Content variant="soft">\n\t\t\t<ContextMenu.Item shortcut="⌘ E">Edit</ContextMenu.Item>\n\t\t\t<ContextMenu.Item shortcut="⌘ D">Duplicate</ContextMenu.Item>\n\t\t\t<ContextMenu.Separator />\n\t\t\t<ContextMenu.Item shortcut="⌘ N">Archive</ContextMenu.Item>\n\n\t\t\t<ContextMenu.Separator />\n\t\t\t<ContextMenu.Item shortcut="⌘ ⌫" color="red">\n\t\t\t\tDelete\n\t\t\t</ContextMenu.Item>\n\t\t</ContextMenu.Content>\n\t</ContextMenu.Root>\n</Grid>
```

----------------------------------------

TITLE: Installing Radix UI Scroll Area Component
DESCRIPTION: Command to install the Scroll Area component from Radix UI using npm.
SOURCE: https://github.com/radix-ui/website/blob/main/data/primitives/docs/components/scroll-area.mdx#2025-04-21_snippet_0

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-scroll-area
```

----------------------------------------

TITLE: Illustrating Radio Button Variants in JSX
DESCRIPTION: This code snippet demonstrates the use of the 'variant' prop to control the visual style of radio buttons. It shows three pairs of radio buttons with different variants: surface, classic, and soft.
SOURCE: https://github.com/radix-ui/website/blob/main/data/themes/docs/components/radio.mdx#2025-04-21_snippet_2

LANGUAGE: jsx
CODE:
```
<Flex align="center" gap="4">
	<Flex gap="2">
		<Radio variant="surface" name="surface" value="1" defaultChecked />
		<Radio variant="surface" name="surface" value="2" />
	</Flex>

	<Flex gap="2">
		<Radio variant="classic" name="classic" value="1" defaultChecked />
		<Radio variant="classic" name="classic" value="2" />
	</Flex>

	<Flex gap="2">
		<Radio variant="soft" name="soft" value="1" defaultChecked />
		<Radio variant="soft" name="soft" value="2" />
	</Flex>
</Flex>
```