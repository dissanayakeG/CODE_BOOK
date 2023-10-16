##Factory Pattern

#####Factory Method is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.

```php
abstract class Dialog {
  public abstract void render();
}

class WindowsDialog extends Dialog {
  public void render() {
    System.out.println("Rendering a Windows dialog");
  }
}

class WebDialog extends Dialog {
  public void render() {
    System.out.println("Rendering a web dialog");
  }
}

class MacDialog extends Dialog {
  public void render() {
    System.out.println("Rendering a Mac dialog");
  }
}

class DialogFactory {
  public static Dialog createDialog(String type) {
    if (type.equals("windows")) {
      return new WindowsDialog();
    } else if (type.equals("web")) {
      return new WebDialog();
    } else if (type.equals("mac")) {
      return new MacDialog();
    } else {
      throw new IllegalArgumentException("Invalid dialog type");
    }
  }
}

Dialog windowsDialog = DialogFactory.createDialog("windows");
windowsDialog.render(); // outputs "Rendering a Windows dialog"

Dialog webDialog = DialogFactory.createDialog("web");
webDialog.render(); // outputs "Rendering a web dialog"

Dialog macDialog = DialogFactory.createDialog("mac");
macDialog.render(); // outputs "Rendering a Mac dialog"

```

###without breaking solid principles

```php
class DialogFactory {
  public static Dialog createWindowsDialog() {
    return new WindowsDialog();
  }

  public static Dialog createWebDialog() {
    return new WebDialog();
  }

  public static Dialog createMacDialog() {
    return new MacDialog();
  }
}

Dialog windowsDialog = DialogFactory.createWindowsDialog();
windowsDialog.render(); // outputs "Rendering a Windows dialog"

Dialog webDialog = DialogFactory.createWebDialog();
webDialog.render(); // outputs "Rendering a web dialog"

Dialog macDialog = DialogFactory.createMacDialog();
macDialog.render(); // outputs "Rendering a Mac dialog"

```

###Pseudocode

```php
class Dialog is
    abstract method createButton()

    method render() is
        Button okButton = createButton()
        okButton.onClick(closeDialog)
        okButton.render()

class WindowsDialog extends Dialog is
    method createButton() is
        return new WindowsButton()

class WebDialog extends Dialog is
    method createButton() is
        return new HTMLButton()

interface Button is
    method render()
    method onClick(f)

class WindowsButton implements Button is
    method render(a, b) is
    method onClick(f) is

class HTMLButton implements Button is
    method render(a, b) is
    method onClick(f) is

class Application is
    field dialog: Dialog

    method initialize() is
        config = readApplicationConfigFile()
        if (config.OS == "Windows") then
            dialog = new WindowsDialog()
        else if (config.OS == "Web") then
            dialog = new WebDialog()
        else
            throw new Exception("Error! Unknown operating system.")

    method main() is
        this.initialize()
        dialog.render()

```
